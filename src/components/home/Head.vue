<template>
    <div id="navi">
        <el-header>
            <el-menu class="el-menu" mode="horizontal" >
                <el-menu-item v-for="item, index in rightmemu" class = "right" :key="index" :index="index.toString()">
                    <a :href="item.href" target="_self" @click="item.click">{{item.label}}</a>
                </el-menu-item>
            </el-menu>
        </el-header>
    </div>
</template>
<script>
    export default {
        name: 'Head',
        props:{
            rightmemu:Array //指定从父组件传输的属性
        },
        install:function(Vue) {
            Vue.component('Head', this);
        }

    }
</script>
<style>
    #navi .el-header {
        z-index: 100;  /*el-diag的遮罩层z-index为2001，这里不能超过它*/
        position: absolute;
        width: 100%;
    }
    #navi li.right:hover {
        color: black;
    }
    #navi li.right {
        float: right!important;
        border: none!important;
        color: white!important;
    }
    #navi ul.el-menu.el-menu--horizontal {
        border-bottom: none;
    }
    #navi .el-menu-item {
        padding:0 10px!important;
    }
    #navi .el-menu-item a {
        text-decoration-line: none;
        padding: 10px;
        border-radius: 5px;
        color:lightgray;
    }
    #navi .el-menu-item a:hover {
        color: white!important;
        /*background-color: var(--vscode-color);*/
    }
    #navi .el-menu {
        background-color: transparent;
    }
    #navi li.el-menu-item:hover {
        background-color: transparent!important;
    }
</style>