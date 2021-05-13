<template>
    <div id="app">
        <el-container>
            <Head></Head>
            <el-main>
                <div id="box">
                    <img id="logo" :src="logourl" />
                    <Search></Search>
                </div>
            </el-main>
        </el-container>
    </div>
</template>

<script>
    import Vue from 'vue'
    import ElementUI from 'element-ui';
    import 'element-ui/lib/theme-chalk/index.css';
    import Search from '@/components/register/Search';
    import Head from '@/components/register/Head';
    Vue.use(ElementUI);//参加官网https://element.eleme.cn/#/zh-CN/component/quickstart
    Vue.use(Search);//注册组件，本质是调用组件的install方法，再在install方法中调用Vue.component()注册组件
    Vue.use(Head);
    export default {
        name: 'App',
        data() {
            return {
                logourl:require('../../assets/logo.png')
            }
        }
    }
</script>

<style>
    :root{
        --border-radius:10px;
        --vscode-color:rgb(87,114,245);
        --font-size:18px;
    }
    div#box {
        width: 50%;
        min-width: 600px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -80%);
    }
    img#logo {
        width: 80%;
        padding: 0 10%;
    }
    body {
        font-family: arial;
    }
</style>
