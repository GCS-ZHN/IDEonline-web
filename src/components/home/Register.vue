<!--
model 配置遮罩层
close-on-click-modal 配置是否点击遮罩就关闭弹窗
-->
<template>
    <div id = "regidiag">
        <el-dialog title="" :visible.sync="isVisible" width="40%" top="7.5vh" :modal="true" :close-on-click-modal="false" center>
            <el-form :model="form" ref="form" :rules="formrules">
                <img id="logo" :src="logourl" />
                <el-form-item label="账号" :label-width="formLabelWidth" prop="account">
                    <el-input v-model="form.account" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="密码" :label-width="formLabelWidth" prop="password">
                    <el-input v-model="form.password" autocomplete="off" show-password></el-input>
                </el-form-item>
                <el-form-item label="确认密码" :label-width="formLabelWidth" prop="confirm">
                    <el-input v-model="form.confirm" autocomplete="off" show-password></el-input>
                </el-form-item>
                <el-form-item label="节点" :label-width="formLabelWidth" prop="node">
                    <el-select v-model="form.node" placeholder="请选择注册节点">
                        <el-option label="CPU" value="cpu"></el-option>
                        <el-option label="GPU" value="gpu"></el-option>
                    </el-select>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="isVisible=false">取消</el-button>
                <el-button type="primary" @click="isVisible=false">注册</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    import $ from 'jquery';
    export default {
        name: 'RegiDiag',
        data() {
            var form = {
                    account:'',
                    node:'cpu',
                    password:'',
                    confirm:''
                };
            return {
                isVisible:false,
                formLabelWidth:"80px",
                form:form,
                formrules: {
                    account:[
                        { required: true, message: '请输入注册账号名', trigger: 'blur' },
                        {
                            trigger:'blur',
                            validator:(rule, value, callback)=> {
                                $.ajax({
                                    url:'/regiCheck',
                                    data:{
                                        info: value
                                    },
                                    method:'get',
                                    datatype:'json',
                                    success:function(data) {
                                        
                                    }
                                });
                            }
                        }
                    ],
                    node:[
                        { required: true, message: '请选择注册节点', trigger: 'blur' }
                        ],
                    password:[
                        {
                            trigger: 'change',
                            validator:(rule, value, callback)=> {
                                if (form.account === '') {
                                    callback(new Error('请先设置用户名'))
                                } else if (value === form.account) {
                                    callback(new Error('不允许与用户名相同'))
                                }
                            }
                        },
                        { required: true, message: '请设置密码', trigger: 'blur' },
                        { min: 6, max: 18, message: '长度在 6 到 18 个字符', trigger: 'blur' }
                    ],
                    confirm:[
                        {
                            trigger:'change',
                            validator:(rule, value, callback)=> {
                                if (form.password === '') {
                                    callback(new Error("请先输入密码"))
                                } else if (value !== form.password) {
                                    callback(new Error("密码不一致"))
                                }
                            }
                        },
                        { required: true, message: '请重复密码', trigger: 'blur' },
                        {}
                    ]
                },
                logourl:require('../../assets/logo.png')
            }
        },
        props: {
            visible: {
                type:Boolean,
                default:false
            }
        },
        watch:{
            visible:{
                handler(){
                    this.isVisible=this.visible
                },
                deep:true,
                immediate:true
            },
            isVisible:{
                handler() {
                    this.$emit('update:visible', this.isVisible)
                },
                deep:true,
                immediate:true
            }
        },
        install:function(Vue) {
            Vue.component(this.name, this);
        }
    };
</script>
<style>
    #regidiag .el-dialog {
        border-radius: 10px!important;
    }
    #regidiag .el-select {
        width:100%;
    }
    #regidiag .el-form-item {
        width: 80%;
        padding: 0 10%;
        margin-bottom: 20px!important;
    }
    #regidiag .el-form-item__label,  #regidiag .el-form-item__content {
        line-height:50px!important;
    }
    #regidiag .el-dialog__body {
        margin-bottom: 100px;
        margin-top: 25px;
        padding: 0;
    }
    #regidiag .el-dialog__header {
        padding: 5px;
    }
    #regidiag .el-dialog__footer {
        padding-bottom: 30px;
    }
    #regidiag .el-button--primary {
        background-color:var(--vscode-color);
    }
    #regidiag .el-button--default:active, #regidiag .el-button--default:hover {
        color: var(--vscode-color);
        border-color: var(--vscode-color);
    }
</style>
