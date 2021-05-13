<template>
    <el-header>
        <el-menu class="el-menu" mode="horizontal" >
            <el-menu-item v-for="item, index in menu.right" class = "right" :key="index" :index="index.toString()"><a :href="item.href" target="_self">{{item.label}}</a></el-menu-item>
        </el-menu>
    </el-header>
</template>
<script>
    export default {
        name: 'Head',
        data() {
            return {
                menu:{
                    right:[
                        {label: "Login", href: "/"},
                        {label: "Manager", href: "/admin"}
                    ]
                }
            }
        },
        install:function(Vue) {
            Vue.component('Head', this);
        }

    }
</script>
<style>
    .el-header {
        z-index: 9999;
        position: fixed;
        width: 100%;
    }

    li.right {
        float: right!important;
    }
    ul.el-menu.el-menu--horizontal {
        border-bottom: none;
    }
    .el-menu-item a {
        text-decoration-line: none;
    }
</style>