# 解决跨域问题的方法(Vue项目用node代理)
## 主要是由于浏览器的同源策略造成的！
## 在登录页面，获取图片验证码
## 1.首先在utils文件内的api.js中进行发送请求设置  
```
//先设置一个前置路径  
let base='';  
//传送json格式的post请求  
export contst postRequest=(url,params)=>{  
  return axios({  
    method:'post',  
    url:` ${base}${url} `,  
    data:params
  })  
}  
```

## 2.在登录页面组件中获取图片验证码
```
//需要引入组件
import {postRequest}  from "../utils/api"
//图片加点击是为了重新加载一个新的
<img :src="captchaUrl" @click="updataCaptchaUrl">
<script>
//此为导出组件
  export deafult{
  name:'Login',
  data(){
     return{
     //captcha为请求路径，加time是为了每次获取的图片都不一样
       captchaUrl:'/captcha?time='+new Date(),
       
     } 
  },
  methods:{
    updataCaptchaUrl(){
     this.capthaUrl='/captcha?time='+new Date();
    }
  }
 }
<script>
```
## 在vue.config.js配置,通过代理，将本地的8080端口的转发到8081
```
let proxyObj={}
proxyObj['/']={
//目标地址
  target:'http://localhost:8081'
//发送请求头host会被设置target
  changePrigin:ture,
//不重写请求地址
  pathReWrite:{
   '^/':'/'
 }
}

module.exports={
   devServer:{
     host:'localhost',
     port:8080,
     //proxy代理
     proxy:proxyObj
   }
 }
```

## 可以将请求以插件的方式引入，这样就不用在每一个组件内import请求了
## 在main.js中进行配置
```
vue.prototype.postRequest=postRequst;
```
## 同理可以将get，put,delete等请求一起配置在此
