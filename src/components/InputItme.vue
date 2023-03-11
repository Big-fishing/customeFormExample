<template>
    <div class="itme3">
        <div v-if="newitmeType.type === 'input'" class="itme-type input">
            <p>{{newitmeType.name}}</p>
            <el-input :placeholder="newitmeType.placeholder"></el-input>
            <span>{{newitmeType.remind}}</span>
            <i class="el-icon-edit-outline" v-on:click="setItme(newitmeType.id)"></i>
            <i class="el-icon-delete-solid" v-on:click="deleteItme(newitmeType.id)"></i>
        </div>
        <div v-if="newitmeType.type === 'password'" class="itme-type password">
            <p>{{newitmeType.name}}</p>
            <el-input :placeholder="newitmeType.placeholder" show-password></el-input>
            <span>{{newitmeType.remind}}</span>
            <i class="el-icon-edit-outline" v-on:click="setItme(newitmeType.id)"></i>
            <i class="el-icon-delete-solid" v-on:click="deleteItme(newitmeType.id)"></i>
        </div>
        <div v-if="newitmeType.type === 'textarea'" class="itme-type textarea">
            <p>{{newitmeType.name}}</p>
            <el-input
            type="textarea"
            :rows="4"
            :placeholder="newitmeType.placeholder">
            </el-input>
            <span>{{newitmeType.remind}}</span>
            <i class="el-icon-edit-outline" v-on:click="setItme(newitmeType.id)"></i>
            <i class="el-icon-delete-solid" v-on:click="deleteItme(newitmeType.id)"></i>
        </div>
        <div v-if="newitmeType.type === 'radio'" class="itme-type radio">
            <p>{{newitmeType.name}}</p>
            <template>
                <el-radio
                v-for="itme in newitmeType.value"
                :key="itme"
                v-model="radio"
                :label="itme">{{itme}}</el-radio>
            </template>
            <i class="el-icon-edit-outline" v-on:click="setItme(newitmeType.id)"></i>
            <i class="el-icon-delete-solid" v-on:click="deleteItme(newitmeType.id)"></i>
        </div>
        <div v-if="newitmeType.type === 'checkbox'" class="itme-type checkbox">
            <p>{{newitmeType.name}}</p>
            <el-checkbox-group v-model="checkedCities">
                <el-checkbox v-for="city in newitmeType.cityOptions" :label="city" :key="city">{{city}}</el-checkbox>
            </el-checkbox-group>
            <i class="el-icon-edit-outline" v-on:click="setItme(newitmeType.id)"></i>
            <i class="el-icon-delete-solid" v-on:click="deleteItme(newitmeType.id)"></i>
        </div>
    </div>
</template>

<script>
export default {
    name:'inputItme',
    data(){
        return{
            radio:'',
            checkedCities:[]
        }
    },
    props:{
        itmeType:Object
    },
    computed:{
        newitmeType(){
            return this.$props.itmeType
        }
    },
    methods:{
        deleteItme(id){
            this.$emit('removEitme',id)
        },
        setItme(id){
            this.$emit('setiditme',id)
        }
    }
}
</script>

<style scoped>
.itme3{
    padding: 10px;
}
.itme3 .itme-type{
    position: relative;
    padding: 20px 5px;
}
.itme3 .itme-type p{
    line-height: 30px;
    font-size: 16px;
}
.itme3 .itme-type span{
    line-height: 16px;
    font-size: 12px;
    opacity: .6;
}
.itme3 .itme-type>i{
    opacity: .3;
    cursor: pointer;
    position: absolute;
    top: 0;
    right: 0;
}
.itme3 .itme-type>i:hover{
    opacity: 1;
    cursor: pointer;
    position: absolute;
    color:red;
}
.itme3 .itme-type .el-icon-edit-outline{
    right: 25px;
}
.itme3 .itme-type:hover{
    border: 1px dotted #333;
}
</style>