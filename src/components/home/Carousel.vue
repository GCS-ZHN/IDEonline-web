<template>
    <div id="caro">
        <el-carousel indicator-position="inside" :autoplay="true" :interval="5000">
            <el-carousel-item v-for="item,index in picset" :key="index">
                <img class="picset" :src="item.url"/>
                <a>{{item.desc}}</a>
            </el-carousel-item>
        </el-carousel>
    </div>
</template>
<script>
    export default {
        name: "caro",
        data:function(){
            return {
                picset:[
                    {
                        desc:"神级代码编辑器Vscode更新3.9.3版",
                        url:require('../../assets/vscode_display.png'),
                    },
                    {
                        desc:"Vscode插件多样, 适合多种语言",
                        url:require('../../assets/vscode_display1.png'),
                    },
                    {
                        desc:"新增支持Jupyter Lab实验室",
                        url:require('../../assets/jupyter_display.png'),
                    },
                    {
                        desc:"Jupyter notebook升级版",
                        url:require('../../assets/jupyter_display1.png'),
                    }
                ]
            };
        },
        install:function(Vue) {
            Vue.component('caro', this);
        }
    }
</script>
<style>
    #caro {
        width: 48vw;
        position: absolute;
        background-color: rgba(255,255,255,0.1);
        top: 250px;
        left: 5%;
        padding:1%;
        border-radius: 10px;
        height: 27vw;
    }
    #caro a {
        bottom: 6%;
        display: block;
        z-index: 3;
        position: absolute;
        left: 50%;
        width: 100%;
        text-indent: 2em;
        transform: translate(-50%, 0);
        background-color: rgba(0,0,0,0.45);
        height: 30px;
        line-height: 30px;
        color: white;
    }
    #caro a:hover {
        text-decoration: underline;
    }
    #caro .el-carousel__container{
        height: 27vw!important;
    }
    #caro .el-carousel__item {
        border-radius: 10px;
    }
    #caro .el-carousel__item img {
        width: 100%;
        position: absolute;
        top:50%;
        left:50%;
        transform: translate(-50%, -50%);
    }
</style>