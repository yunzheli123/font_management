<template>
    <div class = "login_container">
        <!-- 登陆块 -->
        <div class = "login_box">
            <!-- 图片区域 -->
             <div class="avatar_box">
                <img src = "../assets/beauty.jpg" alt />
            </div>
            <!-- 表单区域 -->
            <el-form ref="loginFormRef" :rules="loginrules" :model="loginForm" class="login_form" label-width="0px">
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input el-input v-model="loginForm.username" prefix-icon="iconfont icon-denglu"></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input v-model="loginForm.password" type='password' prefix-icon="iconfont icon-mima"></el-input>
                </el-form-item>
                <!-- 按钮 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginForm()">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
export default{
    data(){
        return{
            loginForm:{
                username:"admin",
                password:"123456"
            },
            loginrules:{
                username:[
                    { required: true, message: '用户名为必填项', trigger: 'blur' },
                    { min: 5, max: 12, message: '长度在 5 到 12 个字符', trigger: 'blur' }
                ],
                password:[
                    { required: true, message: '请输入用户密码', trigger: 'blur' },
                    { min: 6, max: 10, message: '长度在 6~10 个字符', trigger: 'blur' }
                ],
            }
        }
    },
    methods:{
        //重置表单内容
        resetLoginForm(){
            this.$refs.loginFormRef.resetFields();
        },
        //登陆方法
        login(){
            //1.验证校验规则
            this.$refs.loginFormRef.validate(async valid=>{
                if( !valid ) return;
                const {data:res} = await this.$http.post("login",this.loginForm);
                if( res.flag == "ok"){
                    this.$message.success("操作成功 ！！");
                    this.$router.push({path:"/home"});
                    window.sessionStorage.setItem("user",res.user);
                }else{
                    this.$message.error("操作失败");
                }
            })
        }
    },
}
</script>
<style lang = "less" scoped>
.login_container{
    background-color: #2d4b6b;
    height: 100%;
}
.login_box{
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    .avatar_box{
        width: 130px;
        height: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 1px;
        box-shadow: 0 0 2px #ddd;
        position: absolute;
        left: 50%;
        transform: translate(-50%,-50%);
        background-color: #0ee;
        img{
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #eee;
            object-fit: cover;
        }
    }
.btns{
    display: flex;
    justify-content: flex-end;
}
.login_form{
    position: absolute;
    bottom: 0%;
    width: 100%;
    padding: 0 10px;
    box-sizing: border-box;
}
}
</style>