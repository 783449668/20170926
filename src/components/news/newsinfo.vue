<template>
	<div id="tmpl">
		 <div class="title">
            <h4 v-text="info.title"></h4>
            <p>{{info.add_time | datefmt('YYYY-MM-DD')}} {{info.click}}次浏览</p>
        </div>
        <div id="content" class="newsinfo" v-html="info.content">
			
        </div>

        <!--2.0 实现评论组件-->
        <comment :id = "id"></comment>
	</div>
</template>
<script>
	import { Toast } from 'mint-ui';
	import common from '../../kits/common.js';
	import comment from '../subcom/comment.vue';
	export default{

		components:{
			comment  //2.1 注册评论组件
		},
		data(){
			return{
				id:0,
				info:{}
			}
		},
		created(){
			this.id=this.$route.params.id;
			this.getinfo();
		},
		methods:{
			getinfo(){
				var url = common.apisub+'/api/getnew/'+this.id;
				this.$http.get(url).then(function(res){
					var body = res.body;
					if(body.status!=0){
						Toast(body.message)
					}
					console.log(body.message);
					this.info = body.message[0]
				})
			}
		}
	}
</script>

<style scoped>
	.title h4{
        color: #0094ff;
    }
    .title p{
        color:rgba(0,0,0,0.5);
    }

    .title,.newsinfo{
        padding: 5px;
    }
    #content  img {
        border: 0;
        width: 100%;
    }
</style>