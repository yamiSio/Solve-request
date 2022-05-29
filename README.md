##解决跨域问题的方法  
=
##1.首先在api.js中进行发送请求设置  
//先设置一个前置路径  
let base='';  
//传送json格式的post请求  
export contst postRequest=(url,params)=>{  
   return axios({  
   method:'post ,
   url:`${base}${url}`
   })
}
