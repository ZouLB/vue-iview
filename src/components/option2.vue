<template>
	<div>
	    <Table border :loading="loading" :columns="columns7" :data="data6" ref="table"></Table>
	    </br>
	    <Button type="primary" @click="exportData(1)"><Icon type="ios-download-outline"></Icon> 导出数据</Button>
	    <Button type="primary" @click="exportData(2)"><Icon type="ios-download-outline"></Icon> 导出过滤后的数据</Button>
	    <Button type="primary" @click="exportData(3)"><Icon type="ios-download-outline"></Icon> 导出自定义数据</Button>
		<div style="margin: 10px;overflow: hidden">
	        <div style="float: right;">
	            <Page :total="100" :current="1" size="small" show-total show-sizer show-elevator></Page>
	        </div>
	    </div>
	</div>
</template>
<script>
    export default {
        data () {
            return {
                columns7: [
                	{
                        type: 'selection',
                        width: 60,
                        align: 'center'
                    },
                    {
                        title: 'Name',
                        key: 'name',
                        render: (h, params) => {
                            return h('div', [
                                h('Icon', {
                                    props: {
                                        type: 'person'
                                    }
                                }),
                                h('strong', params.row.name)
                            ]);
                        }
                    },
                    {
                        title: 'Age',
                        key: 'age',
                        filters: [
                            {
                                label: 'Greater than 25',
                                value: 1
                            },
                            {
                                label: 'Less than 25',
                                value: 2
                            }
                        ],
                        filterMultiple: false,
                        filterMethod (value, row) {
                            if (value === 1) {
                                return row.age > 25;
                            } else if (value === 2) {
                                return row.age < 25;
                            }
                        }
                    },
                    {
                        title: 'Address',
                        key: 'address',
                        sortable: true
                    },
                    {
                        title: 'Action',
                        key: 'action',
                        width: 150,
                        align: 'center',
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'primary',
                                        size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.show(params.index)
                                        }
                                    }
                                }, 'View'),
                                h('Button', {
                                    props: {
                                        type: 'error',
                                        size: 'small'
                                    },
                                    on: {
                                        click: () => {
                                            this.remove(params.index)
                                        }
                                    }
                                }, 'Delete')
                            ]);
                        }
                    }
                ],
                data6: [
                    {
                        name: 'John Brown',
                        age: 18,
                        address: 'New York No. 1 Lake Park'
                    },
                    {
                        name: 'Jim Green',
                        age: 24,
                        address: 'London No. 1 Lake Park'
                    },
                    {
                        name: 'Joe Black',
                        age: 30,
                        address: 'Sydney No. 1 Lake Park'
                    },
                    {
                        name: 'Jon Snow',
                        age: 26,
                        address: 'Ottawa No. 2 Lake Park'
                    }
                ],
                loading:false
            }
        },
        methods: {
            show (index) {
                this.$Modal.info({
                    title: 'User Info',
                    content: `Name：${this.data6[index].name}<br>Age：${this.data6[index].age}<br>Address：${this.data6[index].address}`
                })
            },
            remove (index) {
                this.data6.splice(index, 1);
            },
            exportData (type) {
                if (type === 1) {
                    this.$refs.table.exportCsv({
                        filename: 'The original data'
                    });
                } else if (type === 2) {
                    this.$refs.table.exportCsv({
                        filename: 'Sorting and filtering data',
                        original: false
                    });
                } else if (type === 3) {
                    this.$refs.table.exportCsv({
                        filename: 'Custom data',
                        columns: this.columns7.filter((col, index) => index < 3),
                        data: this.data6.filter((data, index) => index < 3)
                    });
                }
            }  
        }
    }
</script>
