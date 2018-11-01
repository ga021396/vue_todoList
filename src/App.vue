<template>
  <div id="app">
    <div id="navbar">
    <ul class="navtitle">
        <li v-on:click="changeState(2)" class="navactive"><a href="#">My Tasks</a></li>
        <li v-on:click="changeState(0)"><a href="#">In Progress</a></li>
        <li v-on:click="changeState(1)"><a href="#">Completed</a></li>
    </ul>
</div>
<input id="input" type="text" placeholder=" 	＋ Add Task" v-model="todo">
<input id="new" type="button" value="New" v-on:click="addList">
    
<div class="toDoList">
        <ul id="toDo">
			<div class="todoItem" v-for="( item , index) in showList">
        	<li :class="{onTask : item.toggle,starbackground :item.starbox}">
						<input type="checkbox" v-model="item.toggle">
						<p>{{item.content}}
							<i class="fas fa-pencil-alt" :class="{pencolor : !item.mask}" style="float:right;">
								<input class="starbox" type="checkbox" v-model="item.mask">
							</i>
							<i class="fa-star star" :class="{far :!item.starbox, fas : item.starbox,starcolor:item.starbox}">
									<input class="starbox" type="checkbox" v-model="item.starbox">
							</i>
						</p>
            <p style="margin-top:10px;color:#757575;overflow: hidden;">
							<i class="far fa-calendar-alt icon" :class="{iconmask:!item.deadline}"></i>{{item.deadline}}
			 				<i class="far fa-comment-dots icon" :class="{iconmask:!item.comment}"></i>{{item.comment}}
						</p>
            </li>
			<div class="editList" :class="{mask:item.mask}">
				<div class="edititem">
					<i class="far fa-calendar-alt"></i><p>Deadline</p>
					<input type="date" v-model="item.deadline">
				</div>
				<div class="edititem">
					<i class="far fa-comment-dots"></i><p>Comment</p>
					<textarea placeholder="type your memo here" rows="10" cols="50" v-model="item.comment"/>
				</div>
				<div class="edititem editbtn">
					<button v-on:click="delList(item.id)">Del</button>
					<button v-on:click="addTask(item.id)">Add Task</button>
				</div>
			</div>
			</div>
               <p style="margin-top:10px;color:#C8C8C8; font-style:italic;">{{this.showList.length}} tasks left</p> 
        </ul>
    </div>  
  </div>
</template>

<script>
function addclass(index){
	var navitem=document.getElementsByClassName('navtitle')[0].getElementsByTagName('li');
		for(var i=0;i<3;i++){
			navitem[i].classList.remove("navactive");
	}
	navitem[index].classList.add("navactive");
	//navbar 切換判定
}
function todoalert(){
	var todo=document.getElementById("input");
	todo.classList.add("input-alert");
	//無輸入內容警告
}
function removealert(){
	var todo=document.getElementById("input");
	todo.classList.remove("input-alert");
	//解除無輸入內容警告
}
export default {
  name: 'App',
  data: function () {
    return {
      		todo: 'Hello World!',
			list:[{id:0,content:"todo",toggle:false,starbox:false,mask:true,deadline:"",comment:""}],
			show:2,
			navAct:false,
			key:0
    }
  },
  methods:{
    addList:function(){
			if(this.todo){
				this.key+=1;
				var obj = {id:this.key,content:this.todo,toggle:false,starbox:false,mask:true,deadline:"",comment:""}
				this.list.push(obj);
				this.todo="";
			}
			else{
				todoalert();
			}
			//新增一個TODOLIST
    },
    delList:function(id){
		var delTask = this.list.filter(function(e){
			return e.id!==id;
		})
			this.list=delTask;
			this.showList;
			//刪除一個TODO
		},
	changeState:function(state){
			this.list.sort(function(a,b){
				return b.starbox -a.starbox;
			})
			this.list.sort(function(a,b){
				return a.toggle-b.toggle;
			})
			if(state===0){
			this.show=0;
			addclass(1)
			}
			else if(state===1){
				this.show=1;
				addclass(2)
			}
			else{
				this.show=2;
				addclass(0)
			}
			//改變NAVBAR狀態時
		},
	addTask:function(id){
			var newTask = this.list.find(function(e){
				return e.id===id;
			})
			newTask.mask=true;
			this.showList;
			//完成編輯
		},
	},
	computed:{
		showList:function(){
			if(this.show===2)
			return this.list;
			else if(this.show===1){
			var com =this.list.filter(function(e){
				if(e.toggle===true){
					return e;
				}
			})
			return com;
		}else if(this.show===0){
			var ing = this.list.filter(function(e){
						if(e.toggle===false){
							return e;
						}
					})
					return ing;
		}
		//偵測資料刷畫面
	},
},
watch:{
	todo:function(){
		if(this.todo){
			removealert();
		}
	}//偵測TODO是否為空
}
}
</script>

<style lang="scss">
#app{
  padding: 0;
  margin: 0;
}
body{
    margin: 0;
    padding: 0;
	font-family: 'Open Sans', sans-serif;
	font-weight:600;
}
#navbar{
    width: 100%;
    height: 76px;
    background-color: #4A90E2 ;
	position:relative;
}
#navbar ul{
	position:absolute;
	margin:0;
	padding:0;
	left:50%;
	margin-left:-330px;
	}
#navbar ul li{
    text-align: center;
    display: inline-block ;
    list-style: none;
		font-size:24px;
		text-height:28px;
		width:160px;
		height:76px;
		margin-left:30px;
		margin-right:30px;
		padding-top:23px;
		box-sizing:border-box;
}
a{	
	 text-decoration: none;
	  color:#00408B;
	}
.navactive a{
	color:#FFF;}

#navbar ul li:hover{
		border-bottom:5px solid #00408B;
		
	}
a:hover{
	color:#FFF;
	}
.navactive{
	color:#FFF;
	border-bottom:5px solid #00408B;
	}

input[type="text"]{
	width:620px;
	height:65px;
	position:absolute;
	top:100px;
	left:50%;
	margin-left:-310px;
	font-size:24px;
	line-height:28px;
	border-radius:5px;
	border:2px solid #C8C8C8;
    padding-left: 15px;
    float: left;
	}
.input-alert{
	transition: all .3s;
	border:2px solid red !important;
}
#new{
    position: absolute;
    left:50%;
    top:110px;
    margin-left:350px;
    width:80px;
	padding: 4px;
    height:46px;
    border-radius: 5px;
    font-weight: 600;
    font-size: 20px;
	color:white;
	background:#4A90E2;
	border: #4A90E2;
	transition: all .3s;
	&:hover{
		background:deepskyblue;
		border: deepskyblue;
	}
}
.toDoList{
	width:620px;
	height:auto;
	margin-top: 0;
	position: absolute;
	left:50%;
	margin-left:-310px;
	top:180px;
	}
.toDoList ul {
	padding:0;
	margin:0;
	}
.toDoList ul li{
	width:100%;
	height:102px;
	background-color:#f9f9f9;
	border-radius:5px;
	list-style:none;
	line-height:28px;
	font-size:24px;
	}
.onTask{
	&::before{
		content:'';
		transform: translate(100px,33px);
		position: absolute;
		width: 50%;
		border-bottom: 1px black solid;
	}
	&::after{
		content:'';
		transform: translate(-496px,38px);
		position: absolute;
		width: 50%;
		border-bottom: 1px black solid;
	}
}
input[type="checkbox"]{
	width:24px;
	height:24px;
	margin-top:24px;
	margin-left:32px;
	margin-right:40px;
	margin-bottom:28px;
	float:left;
	}
.toDoList p{
	width:500px;
	float:left;
	padding:0;
	margin:0px;
	margin-top:24px;
	}
i{	
	margin-right:30px;
	}
.star{
	float:right;
	transition: all .1s;
}
.starbackground{
	background-color:#f8d956 !important;
	transition: all .3s;
}
.starcolor{
	color:#F5A623;
	transition: all .3s;
}
.pencolor{
	color:dodgerblue;
	transition: all 1ms;
}
.starbox{
	position:absolute;
	margin: 0 !important;
	padding: 0!important;
	transform: translateX(-26px);
	opacity: 0;
		&:hover{
			cursor: pointer;
		}
}
.editList{
	transition: all .3s;
	height:300px;
	width:620px;
	position: relative;
	border: 1px solid #f9f9f9;
	margin:8px auto;
	background-color:#f9f9f9;
}
.edititem{
	width:80%;
	height:40px;
	margin:10px auto;
	margin-bottom: 40px;
	i{	
		margin-right: 8px;
		float:left;
	}
	input{
		border:white 1px solid;
		height:24px;
	}
	textarea{
		height:96px;
		width:100%;
	}
	p{
		width:100%;;
		float:none;
		padding:0;
		margin:16px;
		margin-top:0px;
	}
}
.editbtn{
	bottom:0px;
	display: flex;
	margin:0;
	padding: 0;
	width:100%;
	position: absolute;
	button{
		box-sizing: border-box;
		height:40px;
		width:50%;
		border: 1px solid #f9f9f9;
		border-radius: 5px;
		background-color: #4A90E2;
		margin:0;
		padding: 0;
		color:white;
		transition: all .3s;
		&:hover{
			cursor: pointer;
		}
	}
	button:first-child{
		background-color:white;
		color:red;
	}
}
.mask{
	display: none;
	}
.todoItem{
	margin-top: 16px;
}
.icon{
	margin:0 12px;
}
.iconmask{
	display:none;
}
</style>
