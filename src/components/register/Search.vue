<template>
    <el-input placeholder="Vscode account" v-model="user" class="input-with-select"  @keyup.enter.native="searchButton">\
        <el-select v-model="node" slot="prepend" placeholder="Node">
            <el-option v-for="v, l in list" :label="l" :value="v" :key="l"/>
        </el-select>
        <el-button slot="append" icon="el-icon-s-promotion"  @click="searchButton">GO</el-button>
    </el-input>
</template>
<script>
    import $ from 'jquery';
    export default {
        name: "Search",
        data: function() {
            return {
                user:"",
                node:"cpu",
                list:{
                    CPU:"cpu",
                    GPU:"gpu"
                }
            }
        },
        methods:{
            searchButton() {
                if (this.user == "") {
                    this.$message({
                        message: 'Please input account',
                        type: 'warning'
                    });
                } else {
                    if (this.node == "") {
                        this.$message({
                            message: "Please select node",
                            type: 'warning'
                        })
                    } else {
                        var form = $("<form method='post'></form>");
                        form.attr({"action":"/check"});
                        var args = {
                            user_node:this.user.toLowerCase()+"-"+this.node
                        };
                        $("body").append(form);
                        $.each(args,function(key,value){
                            var input = $("<input type='hidden'>");
                            input.attr({"name":key});
                            input.val(value);
                            form.append(input);  
                        });
                        form.submit();
                    }
                }
            },
            getArgs(key) {
                var URIargs=window.location.search;
                var argsObj=new Object();
                if (URIargs != "") {
                    URIargs = URIargs.substring(1,URIargs.length);
                    let arr=URIargs.split("&");
                    for (let i in arr) {
                        let tmparr=arr[i].split("=");
                        argsObj[decodeURIComponent(tmparr[0])]=decodeURIComponent(tmparr[1]);
                    }
                }
                return argsObj[key];
            }
        },
        mounted:function () {
            var eid=this.getArgs("eid");
            if (eid=="1") {
                this.$message({
                    message: "Account not found",
                    type: 'error'
                })
            }
        },
        install:function(Vue) {
            Vue.component('Search', this);
        }
    }
</script>
<style>
    :root{
        --border-radius:10px;
        --vscode-color:rgb(87,114,245);
        --font-size:18px;
    }
    .el-input-group__prepend{
        width: 50px!important;
        border-radius: var(--border-radius) 0 0 var(--border-radius);
    }
    .el-input-group__append{
        border-radius: 0  var(--border-radius)  var(--border-radius) 0;
    }
    .el-input-group__prepend, .el-input-group__append{
        background-color:var(--vscode-color);
        color: white;
    }
    .el-input-group__prepend,.el-input__inner, .el-input-group__append .el-button{
        font-weight: bold!important;
        font-size: var(--font-size)!important;
    }
    .input-with-select, .el-input__inner {
        height: 50px!important;
        line-height: 50px!important;
    }
    li.el-select-dropdown__item {
        font-size: var(--font-size);
    }
    li.el-select-dropdown__item.selected {
        color:var(--vscode-color)!important;
    }
    .el-icon-arrow-up:before {
        color: white;
        font-weight: bold;
    }
</style>