<template>
    <div v-if="$props.id">
        <div v-if="state.type === 'input' || state.type === 'password' || state.type === 'textarea'" class="idtype">
            <p>name</p>
            <el-input v-model="name" placeholder="请输入内容"></el-input>
            <p>placeholder</p>
            <el-input v-model="placeholder" placeholder="请输入内容"></el-input>
            <p>remind</p>
            <el-input v-model="remind" placeholder="请输入内容"></el-input>
        </div>
        <div v-else-if="state.type === 'radio' || state.type === 'checkbox'" class="idtype">
            <p>name</p>
            <el-input v-model="name" placeholder="请输入内容"></el-input>
            <p>list</p>
            <draggable v-model="myArray" handle=".mover" animation="100">
                <transition-group>
                    <el-input v-for="(item, index) in myArray" v-model="myArray[index]" :key="index" placeholder="请输入内容">
                        <template slot="prepend"><p class="mover"><i class="el-icon-s-unfold"></i></p></template>
                        <template slot="append"> <p v-on:click="deletelist(index)">删除</p></template>
                    </el-input>
                </transition-group>
            </draggable> 
            <el-input v-model="newtext" placeholder="请输入内容">
                <template slot="append"><p v-on:click="changelist(newtext)">添加</p></template>
            </el-input>
        </div>
    </div>
    <el-empty v-else description="描述文字"></el-empty>
</template>

<script>
import draggable from 'vuedraggable'
export default {
    name:'Details',
    data(){
        return{
            name:'',
            placeholder:'',
            remind:'',
            state:'',
            myArray:[],
            newtext:'',
        }
    },
    props:{
        id:String,
        fromState:Array
    },
    watch:{
        '$props.id'(id){
            const result = this.$props.fromState.filter(citme=>{
                return citme.id === id
            })
            console.log('123456')
            switch (result[0].type) {
                case 'radio':{
                    this.name = result[0].name
                    this.myArray = result[0].value
                    break;
                }
                case 'checkbox':{
                    this.name = result[0].name
                    this.myArray = result[0].cityOptions
                    break;
                }
                default:{
                    this.name = result[0].name
                    this.placeholder= result[0].placeholder
                    this.remind = result[0].remind
                    break;
                }
            }
            this.state = result[0]
        },
        name(str){
            this.$emit('modifyForm',this.$props.id,'name',str)
        },
        placeholder(str){
            this.$emit('modifyForm',this.$props.id,'placeholder',str)
        },
        remind(str){
            this.$emit('modifyForm',this.$props.id,'remind',str)
        },
        myArray(arr){
            if(this.state.type === 'radio') this.$emit('modifyForm',this.$props.id,'value',arr)
            if(this.state.type === 'checkbox') this.$emit('modifyForm',this.$props.id,'cityOptions',arr)
        }
    },
    methods:{
        changelist(text){
            if(!text.length) return this.$message('请输入内容')
            this.myArray.push(text)
            this.newtext = ''
        },
        deletelist(index){
            this.myArray.splice(index,1)
        }
    },
    components:{
		draggable,
    }
}
</script>

<style>
.idtype{
    padding: 10PX;
}
.idtype p{
    margin: 10PX 0;
}
.idtype .el-input-group__append p{
    cursor: pointer;
}
.idtype .mover{
    cursor: pointer;
}
</style>