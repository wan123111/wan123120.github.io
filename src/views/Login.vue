<template>
    <div class="login-wrap">
        <div class="login-form" style="opacity: 0.9;">
            <div class="form-title">
              <img src="../assets/logo.png" style="width: 40px;position: relative; top: 13px;right: 0px"/>
              <span style="color: #cb4e00;font-size: 25px"> 登录课程管理系统 </span>
            </div>
            <el-form
                :model="formData"
                :rules="rules"
                class="form-content"
                label-width="0px"
                ref="form"
            >
                <el-form-item prop="username">
                    <el-row>

                        <el-col :span="24">
                            <el-input
                                placeholder="账号"
                                v-model="formData.username"
                            >
                            </el-input>
                        </el-col>
                    </el-row>
                </el-form-item>

                <el-form-item prop="password">
                    <el-row>

                        <el-col :span="24">
                            <el-input
                                @keyup.enter.native="submit()"
                                placeholder="密码"
                                type="password"
                                v-model="formData.password"
                            >
                            </el-input>
                        </el-col>
                    </el-row>
                </el-form-item>

                <el-form-item
                    prop="userType"
                    label=""
                    label-width="80px"
                    :custom-message="''"
                >
                    <el-radio-group v-model="formData.userType" size="mini">
                        <el-radio-button label="1">学生</el-radio-button>
                        <el-radio-button label="2">教师</el-radio-button>
                        <el-radio-button label="3">管理员</el-radio-button>
                    </el-radio-group>
                </el-form-item>

                <div class="login-btn" v-loading="this.$store.state.loading">
                    <el-button @click="submit()" type="primary">登录</el-button>
                </div>
            </el-form>
        </div>
    </div>
</template>

<script>
import { login } from "../api/user";

export default {
    data: function () {
        return {
            formData: {
                username: "",
                password: "",
                userType: "1",
            },
            rules: {
                username: [
                    {
                        required: true,
                        message: "请输入用户名",
                        trigger: "blur",
                    },
                ],
                password: [
                    { required: true, message: "请输入密码", trigger: "blur" },
                ],
                userType: [
                    {
                        required: true,
                        message: "请选择用户类型",
                        trigger: "blur",
                    },
                ],
            },
        };
    },
    methods: {
        submit() {
            this.$refs.form.validate((valid) => {
                if (valid) {
                    login(
                        this.formData.username,
                        this.formData.password,
                        this.formData.userType
                    ).then((res) => {
                        this.$message.success("登录成功: " + res.username);
                        this.$store.commit("login", res);
                        this.$router.push({ name: "container" });
                    });
                }
            });
        },
    },
};
</script>

<style scoped>
/* .login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background-size: 100% 100%;
  background-color: lightblue;
} */

.login-wrap {
    position: fixed;
    top: -200px;
    left: 0;
    width: 100%;
    overflow-y: auto;
    height: 140%;
    background: url("../assets/05.jpg") center top / cover no-repeat;
}

.form-title {
    width: 100%;
    line-height: 90px;
    text-align: center;
    color: black;
    border-bottom: 1px solid black;
}

.login-form {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 400px;
    margin: -190px 0 0 -175px;
    border-radius: 5px;
    background: white;
    overflow: hidden;
}

.form-content {
    padding: 30px 30px;
}

.login-btn {
    text-align: center;
}

.login-btn button {
    width: 100%;
    height: 36px;
}

.el-radio {
    color: black;
}
</style>
