<template>
	<div>
		<h2>新增礼券包</h2>
		<div class="container">
			<div class="text">基本信息</div>
			<div class="form-box">
				<el-form ref="form" :model="form">
					<el-form-item label="礼券名称 :" style="margin-left: 35%;">
						<el-input v-model="form.name" placeholder="请设置展示给用户看的礼券名称" style="width:220px"></el-input>
					</el-form-item>
					
					<el-form-item label="适用活动类型 :" style="margin-left: 35%;">
                        <el-select v-model="form.activityTypeId" placeholder="所有类型">
                            <el-option v-for="item in typeList" :label="item.name" :value="item.id"></el-option>
                          
                        </el-select>
                    </el-form-item>
				
                    <el-form-item label="礼券面额 :" style="margin-left: 35%;">
                        <el-input v-model="form.money" placeholder="请设置该礼券的面额" style="width:200px"></el-input>&nbsp;&nbsp;元 
                        <span style="display: inline-block;color:red;margin-left: 20px;">最高可设9,999,999元</span>
                    </el-form-item> 
                    
                    <el-form-item label="礼券数量 :" style="margin-left: 35%;">
						<el-input v-model="form.num" style="width:200px"></el-input>
					</el-form-item>    
                    
                    <el-form-item label="活动有效期 :" style="margin-left: 35%;">
                        <el-date-picker v-model="form.startTime" type="datetime" placeholder="选择开始日期时间">
						</el-date-picker>
						<span>&nbsp;&nbsp;~&nbsp;&nbsp;</span>
						<el-date-picker v-model="form.endTime" type="datetime"  placeholder="选择结束日期时间">
						</el-date-picker>
                    </el-form-item>
                    
                    <el-form-item label="活动规则 :" style="margin-left: 35%;">
                        <el-input type="textarea" v-model="form.description" style="width:380px " :rows="5"></el-input>
                    </el-form-item>
					
                    <div class="bottom">
                    	<el-form-item>
	                        <el-button type="primary" @click="onSubmit" size="medium">提交</el-button>
	                        <el-button type="primary" @click="quit" size="medium">返回</el-button>
	                    </el-form-item>
                    </div>
              
                    
                    
                </el-form>
			</div>
		</div>
	</div>
</template>

<script>
	export default{
		name:"added",
		data(){
			return{
				form:{
					
				},
				typeList:[]
			}
		},
		created(){
			//获取活动类型
			this.$axios.get("http://192.168.1.114:9919/productpack/selActivityType")
				.then(res=>{
					//console.log(res.data)
					if(res.data.code==0){
						this.typeList=res.data.data
					}
				})
		},
		methods:{
			//日期
			pickerOptions(){
				
			},
			//提交按钮
			onSubmit() {

					function formatDate(now) {
						var year = now.getFullYear();
						var month = now.getMonth() + 1;
						var date = now.getDate();
						var hour = now.getHours();
						var minute = now.getMinutes();
						var second = now.getSeconds();
return year + "-" + month + "-" + date + " " + hour + ":" + minute;
					}
						
					
					var params = new URLSearchParams();
					params.append('name', this.form.name);
                params.append('activityTypeId', this.form.activityTypeId);
                params.append('num', this.form.num);
                params.append('money', this.form.money);
                params.append('startTime', formatDate(this.form.startTime));
                params.append('endTime', formatDate(this.form.endTime));
                params.append('description', this.form.description);
				this.$axios.post("http://192.168.1.114:9919/productpack/add",params)
						.then(res=>{
							if(res.data.code==0){
								this.$router.go(-1)
							}
						})
			},
			//返回按钮
			quit(){
				this.$router.go(-1)
			}
		}
	}
</script>

<style>
	.container{
		margin-top: 40px;
		height: auto;
		overflow: hidden;
	}
	.text{
		width:100%;
		height: 60px;
		line-height: 60px;
		background: #F0F0F0;
		margin-bottom: 40px;
		color:#999;
		font-size: 18px;
		letter-spacing: 2px;
		text-indent: 16px;
	}
	.form-box{
		width: 100%;
		height: auto;
		overflow: hidden;
	}
	.numbers{
		float: left;
		width: 80px;
		height: 32px;
	}
	.numbers span{
		display: inline-block;
		width: 50px;
		text-align: center;
	}
	.surplus{
		width: 120px;
		float: left;
		margin-left: 100px;
		height: 32px;
	}
	.surplus span{
		display: inline-block;
		width: 50px;
		text-align: center;
	}
	.el-button--warning {
	    color: #fff;
	    background-color: #f56c6c;
	    border-color: #f56c6c;
	    position: relative;
  		left:80px;
  		bottom:33px;
	}
	.el-checkbox__input.is-checked+.el-checkbox__label {
	    color: rgb(50, 65, 87);;
	}
	.el-checkbox__input.is-checked .el-checkbox__inner, .el-checkbox__input.is-indeterminate .el-checkbox__inner {
	    background-color: rgb(50, 65, 87);
	    border-color:rgb(50, 65, 87);
	}
	.el-checkbox__inner:hover {
	    border-color: rgb(50, 65, 87);
	}
	.el-input.is-active .el-input__inner, .el-input__inner:focus {
	    border-color: rgb(50, 65, 87);
	}
	.el-select .el-input__inner:focus {
	    border-color: rgb(50, 65, 87);
	}
	.el-date-table td.today span {
	    color: rgb(50, 65, 87);
	}
	.el-date-table td:hover span{
	    background: rgb(50, 65, 87);
	    color:#fff;
	}
	.el-date-table td.end-date span, .el-date-table td.start-date span {
	    background-color: rgb(50, 65, 87);
	}
	.el-textarea__inner:focus {
	    border-color: rgb(50, 65, 87);
	}
	.left{
		float:left;
		width: 40%;
		margin-left: 100px;
	}
	.right{
		float: left;
		width: 50%;
		margin-left: 50px;
	}
	.el-checkbox{
		margin-left: 40px;
	}
	.el-upload.el-upload--text{
		width: 150px;
		height: 150px;
		float: left;
	}
	.el-icon-plus.avatar-uploader-icon{
		line-height: 150px;
		text-align: center;
	}
	.el-button--danger {
	    color: #fff;
	    background-color: #999;
	    border: none;
	    float: left;
	    margin-top: 55px;
	    margin-left: 20px;
	}
	.el-button--danger:focus, .el-button--danger:hover {
	    background: rgb(50, 65, 87);
	    border-color: rgb(50, 65, 87);
	    color: #fff;
	}
	.bottom-left{
		width: 45%;
		float: left;
	}
	.bottom-right{
		width: 50%;
		float: left;
	}
	.bottom{
		width: 100%;
		height: auto;
		overflow: hidden;
		text-align: center;
		margin-top: 100px;
		float: left;
	}
	.target{
		width: 100%;
		height: auto;
		overflow: hidden;
		margin-top: 50px;
		margin-bottom: 30px;
	}	
</style>