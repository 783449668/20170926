<template>
	<div id="tmpl">
		 <!--1.0 实现的是图片详情和缩略图-->
		<div id="desc">
			<!--图片详情- 标题部分-->
			<div class="title">
				<h4>{{photoinfo.title}}</h4>
				<p>
					{{photoinfo.add_time | datefmt('YYYY-MM-DD HH:mm:ss')}}  {{photoinfo.click}}次浏览
				</p>
				<p class="line"></p>
			</div>
		
		</div>

	

			
			<!--缩略图-->
			<div class="mui-content">
				<ul class="mui-table-view mui-grid-view mui-grid-9">
					<li  v-for="(item, index) in list" class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
						<img class="preview-img"  :src="item.src" height="100" @click="$preview.open(index, list)"
								 >
					</li>
				</ul>
			</div>

				<!--图片详情- 摘要部分-->
			<p v-html="photoinfo.content"></p>

				<!--2.0。3 集成评论组件-->
		<div id="comment">
			<comment :id="id"></comment>
		</div>

	</div>
</template>

<script>
	import comment from '../subcom/comment.vue';
	import common from '../../kits/common.js';
	import {Toast} from 'mint-ui';
	export default{
			components:{
				comment 
			},
			data(){
				return{
					id:'',
					photoinfo:{},
					list:[
					]
				}

			},
			created(){
				this.id = this.$route.params.id;
					this.getinfo();
					this.getimgs();
			},
			methods:{
				getinfo(){
					var url = common.apisub+'/api/getimageInfo/'+this.id;
					this.$http.get(url).then(function(res){
						var body = res.body;
						if(body.status!=0){
							Toast(body.message);
						}
						this.photoinfo=body.message[0];
					})

				},
				getimgs(){
					var url = common.apisub+'/api/getthumimages/'+this.id;
					this.$http.get(url).then(function(res){
						var body = res.body;
						if(body.status!=0){
							Toast(body.message);
							return;
						}
						body.message.forEach(function(item){
							var img = document.createElement('img');
							img.src =  item.src;
							item.h = img.height;
							item.w = img.width;
						});
						this.list = body.message;

						
					})
				}
			}
		
	}
</script>
<style scoped>
	 /*图片详情样式*/
  #desc{
	  padding: 10px;
  }
	#desc  .title h4{
		color: #0094ff;
	}

  #desc .title p{
	color:rgba(0,0,0,0.4);
	  margin-top: 10px;
  }

  #desc .title .line{
	  width: 100%;
	  height: 1px;
	  border-bottom: 1px solid rgba(0,0,0,0.4);
  }

  .mui-content,.mui-content ul{
	  background-color: #fff;
  }
  .mui-grid-view.mui-grid-9 .mui-table-view-cell{
	  border-right:0px;
	  border-bottom:0px;
  }
  .mui-grid-view.mui-grid-9{
	  border-top:0px;
	  border-left:0px;
  }

  /* 9宫格缩略图的样式*/
	.mui-content img{
		width: 100px;
		height: 100px;
	}
</style>