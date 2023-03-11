<template>
	<div class="box">
		<div class="left">
			<el-tabs v-model="activeName" type="card">
				<el-tab-pane label="标签" name="label">
					<div class="tabs-List">
						<draggable
						:list="tabsList"
                        :clone="clone"
						v-bind="{ group: { name: 'itxst', pull: 'clone' }, sort: true }"
						animation="300"
						:move="onMove">
							<transition-group>
								<span v-for="itme in tabsList" :key='itme.type'>
									{{itme.type}}
								</span>
							</transition-group>
						</draggable>	
					</div>
				</el-tab-pane>
				<el-tab-pane label="标签管理" name="details">
					<Details
                    :fromState='fromState'
                    :id='formid'
                    @modifyForm='modifyForm'/>
				</el-tab-pane>
			</el-tabs>
		</div>
		<div class="right">
			<h3>Form</h3>
            <div class="content">
                <draggable
                :list="fromState"
                group="itxst"
                animation="300"
                :move="onMove">
                    <transition-group class="right-content">
                        <InputItme
                        v-for="itme in fromState"
                        :key="itme.type+Math.random()*15"
                        :itmeType='itme'
                        @removEitme='removEitme'
                        @setiditme='(id)=>{
                            formid=id
                            activeName="details"
                        }'/>
                    </transition-group>
                </draggable>
                <el-button class="from-submit" v-if="fromState.length">点击提交</el-button>
            </div>
            <el-empty v-if="fromState.length === 0" description="暂无状态"></el-empty>
		</div>
	</div>
</template>

<script>
import InputItme from '@/components/InputItme.vue'
import Details from '@/components/Details.vue'
import draggable from 'vuedraggable'
import {nanoid} from 'nanoid'

export default {
    name:'CardContent',
	data(){
		return{
            formid:'',
			activeName:'label',
			fromState:[],
			tabsList:[
				{
					type:'input',
					name:'',
					placeholder:'',
					remind:''
				},
				{
					type:'password',
					name:'',
					placeholder:'',
					remind:''
				},
				{
					type:'textarea',
					name:'',
					placeholder:'',
					remind:''
				},
				{
					type:'radio',
					name:'',
					value:[
						'备选项1',
						'备选项2'
					]
				},
				{
					type:'checkbox',
					name:'',
					cityOptions :['上海', '北京', '广州', '深圳'],
				},
			]
		}
	},
	components:{
		draggable,
        InputItme,
        Details
	},
	methods: {
      //move回调方法
		onMove(e){
			const itme = e.related.className
			if(itme === 'itme3' || itme === 'right-content') return true
			return false;
		},
        clone(origin){
            let data = JSON.parse(JSON.stringify(origin))
            data.id = nanoid()
            return data
        },
        removEitme(id){
            this.fromState = this.fromState.filter(itme=>{
                return itme.id !== id
            })
        },
        modifyForm(id,type,value){
            this.fromState.forEach((itme,index)=>{
                if(itme.id === id){
                    this.fromState[index][type] = value
                }
            })
        }
	},
}
</script>

<style>
.box{
	overflow: hidden;
	display: flex;
	width: 1200px;
	height: 500px;
	margin:15px auto 0;
	justify-content:space-between;
}
.box>div{
	flex: 1;
	margin: 10px;
	box-shadow: rgb(0,0,0,.3) 2px 2px 5px;
	border-radius: 5px;
}
.left{
	overflow: hidden;
	background:#fff;
}
.left .tabs-List div span{
	display: flex;
	padding: 10px;
}
.left .tabs-List div span span{
	cursor: pointer;
	margin: 5px;
	padding:  0 10px;
	line-height: 30px;
	background: #6c1dea;
	color: #fff;
}
.left .el-tabs__nav-wrap{
	background: #383dc0;
}
.left .el-tabs--card > .el-tabs__header .el-tabs__nav{
	display: flex;
	width: 100%;
}
.left .el-tabs .el-tabs__nav-scroll .el-tabs__nav .el-tabs__item{
	padding: 0;
	text-align: center;
	flex:1;
	color: #fff;
}	
.left .el-tabs--card > .el-tabs__header .el-tabs__item.is-active{
	background: #fff;
	color: #000;
}

.right{
    position: relative;
	overflow: hidden;
	background:#fff;
}
.right > h3{
	line-height: 40px;
	border-bottom:solid 1px #cbcbcb;
	text-indent: 15px;
}
.right .sortable-ghost{
    position: relative;
    display: block;
    width: 100%;
    line-height: 50px;
    text-align: center;
    font-size: 16px;
    color: #000;
    padding-bottom: 5px;
    background: rgb(160, 134, 245)
}
.right .sortable-ghost::before{
    content: '+';
    position: absolute;
    top:15px;
    left: 0;
    right: 0;
    color: #000;
}
.right .itme3.sortable-ghost{
    opacity:.3;
    background: #fff;
}
.right .from-submit{
    margin-left:15px ;
}
.right .content{
    overflow: auto;
    height: 424px;
    padding-bottom: 15px;
    z-index:1000;
    position: relative;
}
.right .content>div>.right-content{
    display: block;
    min-height: 300px;
}
.right .el-empty{
    z-index: 10;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
}
</style>