<template>
	<div>
		<Dropdown>
        	<Button type="primary">
            	切换语言:{{currentLanguage}}
                <Icon type="ios-arrow-down"></Icon>
            </Button>
            <DropdownMenu slot="list">
                <!-- 
                    DropdownItem 这里必须用 @click.native 绑定点击事件 
                 -->
                <DropdownItem v-for="(item,key) in language" :key="item.id" @click.native="changeLanguage(key)">
                    {{ item }}
                </DropdownItem>
            </DropdownMenu>
        </Dropdown>
        
        	{{$t('home')}}
	</div>
</template>

<script>
    import Vue from "vue";
	export default {
	  mounted() {
	    this.currentLanguage = this.language[Vue.config.lang];
	  },
	  data() {
	    return {
	      currentLanguage: "",
	      language: {
	        "zh-CN": "中文",
	        "en-US": "English"
	      }
	    };
	  },
	  methods: {
	    changeLanguage: function(key) {
	      Vue.config.lang = key;
	      this.currentLanguage = this.language[key];
	    }
	  }
	};
</script>