<template>
    <div id="app">
        <img id="logo" :src="logourl" />
        <caro />
        <el-container>
            <Head :rightmemu="rightmemu"></Head>
            <el-main>
                <div id="box">
                    <login></login>
                </div>
            </el-main>
        </el-container>
        <RegiDiag :visible.sync="regiDiagVisible" />
    </div>
</template>

<script>
    import Vue from 'vue'
    import ElementUI from 'element-ui';
    import 'element-ui/lib/theme-chalk/index.css';
    import login from '@/components/home/Login';
    import Head from '@/components/home/Head';
    import RegiDiag from '@/components/home/Register';
    import caro from '@/components/home/Carousel';
    Vue.use(ElementUI);//参加官网https://element.eleme.cn/#/zh-CN/component/quickstart
    Vue.use(login);//注册组件，本质是调用组件的install方法，再在install方法中调用Vue.component()注册组件
    Vue.use(Head);
    Vue.use(RegiDiag);
    Vue.use(caro);
    export default {
        name: 'App',
        data() {
            return {
                logourl:require('../../assets/logo.png'),//相对当前js文件，并且会被资源打包，与static不同。./static/img/logo.png相对的是项目根目录
                rightmemu:[
                    {label: "注册", href: "javascript:void(0)", click:()=>{
                        /*this.regiDiagVisible=true*/
                        this.$message({
                            message: "请联系管理员注册，自主注册暂不支持"
                        })
                    }},
                    {label: "管理员", href: "javascript:void(0)", click:()=>{
                        this.$message({
                            message: "你不是管理员，勿扰",
                            type: 'warning'
                        })
                    }},
                    {label: "帮助", href: "javascript:void(0)", click:()=>{
                        this.$message({
                            message: "我帮不了你，对不起",
                            type: 'warning'
                        })
                    }}
                 ],
                 regiDiagVisible:false
            }
        }
    }
</script>

<style>
    :root{
        --border-radius:10px;
        --vscode-color:#5772F5;
        --dark-red:#961618;
        --light-red:#C81618;
        --font-size:18px;
    }
    div#box {
        background-color: rgba(255, 255, 255, 0.25);
        border-radius: 10px;
        width: 30vw;
        height:27vw;
        min-height: 250px;
        min-width: 250px;
        padding-top: 7vw;
        padding-bottom: 0;
        position: absolute;
        top: 120px;
        right: 5%;
        margin-bottom: 100px;
    }
    img#logo {
        width: 35%;
        position: absolute;
        left: 0;
    }
    body {
        font-family: arial;
        background-image: url(../../assets/cover.jpeg);
        background-size: 100% 100vh;/*100vh eq 100% height*/
        background-position-x:center;
        min-height: 610px;
    }
@media screen and (max-width:350px) {
    div#box {
        width:95%;
    }
    img#logo {
        left: 50%;
        transform: translate(-50%, 30px);
    }
}
@media screen and (max-width: 1260px) {
    div#box {
        right:50%;
        transform: translate(50%, 0);
        max-width:350px;
        width:90%;
        min-width:0;
        padding-top:50px;
        padding-bottom:50px;
    }
    div#caro {
        display:none;
    }
    img#logo {
        width:250px;
    }
}
</style>
