<template>
<div  id="PositionCard">
	    <div v-for="item in positionCardList">
	        <div class="PositionCard-info" align="left">
			    <router-link :to="{ name: 'Position', query: { id:idMsg,companyName: item.companyName,positionName:item.positionName }}"><div class="position-panel"><!-- 这是左层div -->
			    		<h4>{{item.positionName}}</h4>
	
			    		<div class="middle"><!-- 这是左侧中层div -->
			    				<span class="salary">{{item.salary}}</span>
			    				<span>{{item.city}}</span>
			    				<span>{{item.requiredEducation}}</span>
			    				<span>{{item.requiredExperience}}</span>
			    		</div class="footer">
			    		<div class="feedback"><!-- 左侧下层div -->
			    			<span>{{item.releaseTime}}</span>
			    			<span>{{item.positionType}}</span>			
			    		</div>
			    </div></router-link><div class="company-panel" v-if="(item.companyName!=urlCompanyName)&&(caller=='applicant')">
			    <!-- 这是右层div -->
			    		<div><!-- 那个公司名称的div -->
			    			<p>{{item.companyName}}</p>
			    		</div>
			    		<div align="left">
			    			<p>
			    				{{item.officeHour}}
			    			</p>
			    		</div>
			    </div><div class="company-panel" v-if="caller=='hr'">
			    		<button class="btn-red del-btn" v-on:click="deletePosition(item.positionName)">删除</button>
			    </div>


		    </div>
		</div>
    
</div>	

</template>

<style type="text/css" scoped>
.PositionCard-info{
	cursor: pointer;
	box-sizing:border-box;
	width: 100%;
	/*border:1px solid #dfdfdf;*/
	box-shadow: 1px 1px 5px 1px #dfdfdf;
	margin-top:0.5em;
	margin-bottom: 0.5em;
	padding: 1em;
	height: 8em;
	overflow: hidden;
	transition: all 0.6s;
}
.del-btn{
	border:none;
	position: absolute;
	width: 8em;
	height: 3em;
	font-size: 14px;
	border-radius: 10px;
	margin:auto auto;
	left: 0;
	right: 0;
	top: 0;
	bottom: 0;
}
.btn-red{
	transition: all 0.6s;
	background-color: inherit;
	color: #ff6666;
}
.btn-red:hover{
	background-color: #ff6666;
	color: white;
}
.PositionCard-info:hover{
	/*border:1px solid orange;*/
	position: relative;
	box-shadow: 1px 1px 5px 1px orange;
}
.position-panel{
	display: inline-block;
	width: 60%;
	vertical-align: top;
}
h4{
	margin: 0.5em;
	color: #eeaaaa;
	font-weight: bold;
}
.salary{
	color: orange;
}
.middle{
	overflow: hidden;
}
.footer{
	overflow: hidden;
}
.company-panel{
	display: inline-block;
	position: relative;
	height: 100%;
	width: 40%;
	vertical-align: top;
}
.feedback{
	margin-top: 0.5em;
}
span{
	padding-left:0.5em;
	padding-right:0.5em;

	float: left;
	display: inline-block;
	width: auto;
	border-right: 1px solid #dfdfdf;
}

</style>
<script type="text/javascript">
import router from '../main.js'

export default{
	name:'PositionCard',
	props:['positionCardList','caller'],
	data(){
		return{
			idMsg:this.$router.currentRoute.query.id,
			nameMsg:"",
			urlCompanyName:this.$router.currentRoute.query.companyName,
			storagedCompanyName:window.localStorage.getItem(this.idMsg),
		}
	},
	methods:{
		deletePosition:function(posName){
			if(confirm("确认删除"+posName+"?")){
				var jsonObj={
					'id':window.localStorage.getItem("id"),
					'companyName':this.storagedCompanyName,
					'positionName':posName,
				};
				$.ajax({
					url:'./src/api/dropPosition',
					data:JSON.stringify(jsonObj),
					dataType:'json',
					type:'post',
					success:(result)=>{
						if(result.code==0){
							alert("删除成功");
							location.replace(location.href)
						}else{
							alert(result.code+" "+result.message);
						}
					},
					error:function(result,msg,error){
						alert("服务器异常")
					}
			})
			}
		}
	},
	created:function(){
			this.nameMsg=window.localStorage.getItem(this.idMsg);
	}
}
</script>
