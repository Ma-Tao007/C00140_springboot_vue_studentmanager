# C00140_springboot_vue_studentmanager

@[TOC](基于Springboot+Vue的前后端分离的学生信息管理系统)
## 项目简介
本项目为基于Springboot+Vue的前后端分离的项目，本项目分为三种权限：管理员、教师和学生
管理员功能：
登录、学院管理、班级管理、课程管理、学生管理、教师管理、修改密码；

教师功能：
登录、学院查看、班级查看、课程查看、学生管理、个人信息修改、修改密码；

学生功能：
登录、学院查看、班级查看、课程查看、教师查看、个人信息修改、修改密码；


## 项目地址
> [源码地址](http://www.manoncode.cn/details?id=140)

 
## 开发环境

运行环境：推荐jdk1.8；
开发工具：eclipse以及idea（推荐）、vscode、node环境（16.X）；
操作系统：windows 10 8G内存以上（其他windows以及macOS支持，但不推荐）；
浏览器：Firefox(推荐)、Google Chrome(推荐)、Edge;
数据库：MySQL8.0(推荐)及其他版本（支持，但容易异常尤其MySQL5.7（不含）以下版本）；
数据库可视化工具：Navicat Premium 15（推荐）以及其他Navicat版本
是否maven项目：是

>登录：
管理员：用户名：admin 密码：123456
教师：用户名：JS1240110 密码：123456
学生：用户名：JV20002 密码：123456
>
>注意：后台运行后端口必须为8080（不用改，默认就是，如果改动的话需要在前台代码的axios中也需要同步修改）

## 项目技术
 
后端：SpringBoot、Mybatis、mysql
前端：Vue3、axios、TypeScript、ElementUI

## 运行截图



  1.功能结构 

![1.功能结构](https://img-blog.csdnimg.cn/img_convert/5807d17664840afd582706d14db7045d.png)

  -1.下载所得 

![-1.下载所得](https://img-blog.csdnimg.cn/img_convert/0d560874c9a306705182272a6a9237b4.png)

  2.项目结构 

![2.项目结构](https://img-blog.csdnimg.cn/img_convert/258d823435a05b335b73633da2704003.png)

  3.数据库模型 

![3.数据库模型](https://img-blog.csdnimg.cn/img_convert/3ede44979dec3a6478e51e7cbbd34ba4.png)

  4.数据库 

![4.数据库](https://img-blog.csdnimg.cn/img_convert/339930bb6baa715ff46779bc49fcdae8.png)

  5.登录 

![5.登录](https://img-blog.csdnimg.cn/img_convert/2c25f92935302b866336dcb2d84afc1a.png)

  6.首页 

![6.首页](https://img-blog.csdnimg.cn/img_convert/d529a1e71a9db7954368b8e18e516097.png)

  7.学院管理 

![7.学院管理](https://img-blog.csdnimg.cn/img_convert/4bdeb54e39bbf650189698758536b62d.png)

  8.班级管理 

![8.班级管理](https://img-blog.csdnimg.cn/img_convert/8bd8ab3185e5b6a249ca246dd2b00a15.png)

  9.课程管理 

![9.课程管理](https://img-blog.csdnimg.cn/img_convert/9f4b7a2e12e991cb564cad09da4b9c36.png)

  10.教师管理 

![10.教师管理](https://img-blog.csdnimg.cn/img_convert/8543368d3d2a7fcfa90bea3194e8dbd8.png)

  11.学生管理 

![11.学生管理](https://img-blog.csdnimg.cn/img_convert/6b786835fa4e095514328ad680f0e6b2.png)

  12.修改密码 

![12.修改密码](https://img-blog.csdnimg.cn/img_convert/0d06b7f17cdc2a706f724afe221ebec7.png)

  13.个人信息修改 

![13.个人信息修改](https://img-blog.csdnimg.cn/img_convert/104da271822dcfd7ce38783b854f7387.png)

  14.教师和学生查看页面等 

![14.教师和学生查看页面等](https://img-blog.csdnimg.cn/img_convert/d53c9c0ac45d557ad4f86f9be34ff5a9.png)
## 部分代码

```typescript
<template>
    <div class="lg-contain">
        <h1 class="h_header">欢迎来到学生信息管理系统</h1>
        <el-form ref="ruleFormRef" :model="ruleForm" status-icon :rules="rules" label-width="120px" class="demo-ruleForm">
            <el-form-item label="用户名" prop="username">
                <el-input v-model="ruleForm.username" type="text"  placeholder="请输入用户名"/>
            </el-form-item>
            <el-form-item label="密码" prop="password">
                <el-input v-model="ruleForm.password" type="password" placeholder="请输入密码" />
            </el-form-item>
            <el-form-item label="角色" prop="type">
                <el-radio-group v-model="ruleForm.type">
                    <el-radio label="1">管理员</el-radio>
                    <el-radio label="2">教师</el-radio>
                    <el-radio label="3">学生</el-radio>
                </el-radio-group>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm(ruleFormRef)">提交</el-button>
                <el-button @click="resetForm(ruleFormRef)">重置</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script lang="ts" setup>

import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { login } from '@/request/common';
import { ElMessage } from 'element-plus'
import {useRouter} from 'vue-router';

const ruleFormRef = ref<FormInstance>()
//定义登录元素组件
const ruleForm = reactive({
    username: '',
    password: '',
    type:"1",
})
//校验
const rules = reactive<FormRules>({
    username: [
        { required: true, message: '请输入用户名', trigger: 'blur' }
    ],
    password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
})
const router = useRouter();
//提交表单
const submitForm = (formEl: FormInstance | undefined) => {
    if (!formEl) return
    formEl.validate((valid) => {
        if (valid) {
            login(ruleForm).then(res=>{
                if(res.success){
                    ElMessage({
                        message: '登录成功',
                        type: 'success',
                    })
                    res.data.type = ruleForm.type;
                    console.log(res);
                    
                    //将登录信息保存在缓存中
                    localStorage.setItem("sessionUser",JSON.stringify(res.data.sessionEntity))
                    localStorage.setItem("token",res.data.token);
                    
                    //跳转首页
                    router.push("/")
                }else{
                    ElMessage.error(res.msg)
                }
               
            }).catch(err=>{
                console.log(err);
                
            });
        } else {
            return false
        }
    })
}
//重置表单
const resetForm = (formEl: FormInstance | undefined) => {
    if (!formEl) return
    formEl.resetFields()
}

</script>

<style scoped lang="scss">
.lg-contain{
    background: url("../../assets/login.jpg");
    width: 100%;
    height: 100%;
    padding:0.1px;
    background-size: cover;
}
.demo-ruleForm{
    background-color: #ffff;
    width: 500px;
    padding:50px;
    border-radius: 20px;
    margin: auto;
}
.h_header{
    text-align: center;
    margin:150px auto 80px;
}
</style>
```

