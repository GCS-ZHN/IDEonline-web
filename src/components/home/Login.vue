<template>
    <div id="login">
        <el-form :model="formData" :rules="formRules" ref="formData" label-position="top">
            <el-form-item prop="user">
                <el-input v-model="formData.user" placeholder="用户名"></el-input>
            </el-form-item>
            <el-form-item prop="au">
                <el-input v-model="formData.au" placeholder="密码" show-password></el-input>
            </el-form-item>
            <el-form-item>
                <div id = "node">
                    <!--el-tooltip class="item" effect="dark" content="服务器节点" placement="top"-->
                        <el-select v-model="formData.node" placeholder="请选择节点">
                            <el-option v-for="item, index in nodeset" :key="index" :value="item" :label="item"></el-option>
                        </el-select>
                    <!--/el-tooltip-->
                </div>
                <div id = "plt">
                    <!--el-tooltip class="item" effect="dark" content="开发平台" placement="top"-->
                        <el-select v-model="formData.platform" placeholder="请选择平台">
                            <el-option v-for="item, index in platform" :key="index" :value="item" :label="item"></el-option>
                        </el-select>
                    <!--/el-tooltip-->
                </div>
            </el-form-item>
            <div id="submit">
                <el-form-item>
                    <el-button type="primary" icon="el-icon-s-promotion" @click="loginButton()">登录</el-button>
                </el-form-item>
            </div>
        </el-form>
    </div>
</template>
<script>
    import $ from 'jquery';
    import Cookies from 'js-cookie';
    import JSEncrypt from 'jsencrypt';
    let Base64 = require('js-base64').Base64;
    export default {
        name: "login",
        data: function() {
            return {
                formData:{           //登录表单数据 
                    user:"",
                    au:"",
                    node:"node210",
                    platform:"vscode"
                },
                nodeset:["node1", "node2", "node3", "node12", "node41", "node210"],
                platform:["vscode", "jupyter"],
                auflag:0,            //登录验证的结果反馈值，登录时获取
                pubkey:undefined,    //RSA加密公钥，vm挂载时获取（vue钩子函数mounted）
                formRules:{          //表单校对规则
                    user:[
                        { required: true, message: '用户名不能为空!', trigger: 'blur' },
                        {validator:(rule, value, callback)=>{
                            if(this.auflag === -1) {
                                callback(new Error("用户不存在"))
                            }
                        }, trigger: 'blur'}
                    ],
                    au:[
                        { required: true, message: '密码不能为空!', trigger: 'blur'},
                        {validator:(rule, value, callback)=>{
                            if(this.auflag === 1) {
                                callback(new Error("密码错误"))
                            }
                        }, trigger: 'blur'}
                    ]
                }
            }
        },
        methods:{
            /*
            * POST data of account and node, the action of login Button
            */
            loginButton() {
                let flag = false;
                this.$refs["formData"].validate((val)=>{
                    flag = !val;
                });
                if (flag||this.formData.au === '') {
                    return;
                } else {
                    let vm = this;
                    if (this.pubkey !== undefined) {
                        //利用公钥对密码进行RSA加密
                        let encrypt = new JSEncrypt();
                        encrypt.setPublicKey('-----BEGIN PUBLIC KEY-----' + this.pubkey + '-----END PUBLIC KEY-----');
                        let pwd = encrypt.encrypt(this.formData.au);
                        $.ajax({
                            url:"/validate",
                            method:"post",
                            data:{
                                user: this.formData.user,
                                au: pwd,
                                node: this.formData.node.substring(4)
                            },
                            datatype:"json",
                            success:function(data) {
                                vm.auflag = data.status;
                                vm.$refs["formData"].validate((val)=>{});
                                switch (vm.auflag) {
                                    case 0: {
                                        Cookies.set('auth', 
                                            Base64.encode(
                                                vm.formData.user+":"+
                                                vm.formData.au+":"+
                                                vm.formData.node+":"+
                                                vm.formData.platform
                                            ),
                                            {expires: 7, path: window.location.pathname});
                                        window.location.href = "/"+vm.formData.platform;
                                        break;
                                    }
                                    case 2: {
                                        vm.$message({
                                            message: "未知错误，请联系管理员",
                                            type: "error"
                                        });
                                        break;
                                    }
                                    case 3: {
                                        vm.$message({
                                            message: "页面可能闲置过久，请尝试重新打开页面",
                                            type:"error"
                                        });
                                        break;
                                    }
                                    case 4: {
                                        vm.$message({
                                            message: "非法请求",
                                            type:"error"
                                        });
                                        break;
                                    }
                                }
                            }
                        })
                    } else {
                        alert("no pubkey");
                    }
                }
            },
        },
        /*
        * Install function of this VUE component, it invoked by 'Vue.use' to register and initialize this component
        * @param Vue the type object of class 'Vue'
        */
        install:function(Vue) {
            Vue.component('login', this);
        },
        /*
        * 当Vue对象被挂载时，获取cookie中记住密码的信息以及获取登录公钥。
        */
        beforeMount:function() {
            let auth = Cookies.get("auth");
            if (auth !== undefined) {
                let arr = Base64.decode(auth).split(":");
                if (arr.length != 4) return;
                this.formData.user=arr[0];
                this.formData.au=arr[1];
                this.formData.node=arr[2];
                this.formData.platform=arr[3];
            };
            let vm = this;
            $.ajax({
                url:"/getkey",
                method:"get",
                datatype:"json",
                success:function(data) {
                    vm.pubkey = data.key;
                }
            });
        }
    }
</script>
<style>
    #login .el-button{
        font-weight: bold!important;
        font-size: var(--font-size)!important;
        border-color:transparent;
        color:lightgray;
        background-color: var(--dark-red);
        width:100%;
    }
    #login .el-button:hover{
        color:white;
        background-color: var(--light-red);
    }

    #login .el-form {
        height: 0;
        width: 100%;
    }
    #login .el-form .el-form-item{
        padding-left: 12%;
        padding-bottom: 10px;
        width: 76%;
    }
    #node, #plt {
        width: 45%;
    }
    #node {
        float: left;
    }
    #plt {
        float: right;
    }
    #login .el-input {
        font-size: var(--font-size);
    }
    #login .el-input input {
        background-color:transparent;
    }
    #login .el-input__inner {
        color: white;
    }

    /**下拉选项配置*/
    .el-select-dropdown.el-popper {
        background-color: rgba(255,255,255,0.5);
    }
    .el-select-dropdown__item{
        font-size:18px!important;
    }
    .el-select-dropdown__item.selected {
        color:var(--light-red);
        font-weight:bold;
    }
    .el-select-dropdown__item.hover, .el-select-dropdown__item:hover {
        background-color: rgba(255,255,255,0.61)!important;
    }
    .el-form-item__error {
        color: white!important;
        font-weight: bold;
    }
    #submit {
        /*padding-top: 100px;*/
    }
</style>