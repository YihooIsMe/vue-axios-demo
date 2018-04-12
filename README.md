# vue-axios-demo以及nginx反向代理实现跨域请求

##nginx配置文件相关设置详见nginxConf/nginx.conf
    1. 区分请求的接口是http协议还是https协议，在相应的模块中配置配置相关参数
    2. 特别注意server下的配置 
        1. listen:监听的端口；server_name:服务器的名字，就是通过反向代理大家能访问的地址
        2. location / {
                root vuejs-news-master/vuejs-news-master; #需要访问的路径，相对于nginx.exe
                index index.html index.htm;               #默认打开的文件名称
        }
        3. location /apis {
                rewrite  ^/apis/(.*)$ /$1 break;                        #反向代理，这里我也不是很理解，但是差不多是固定的写法
                proxy_pass   http://www.******.com.cn:9099/;            #请求接口的地址
            }
            
##app.js下的axios请求数据的时候url的写法就要改变
    1. axios.get('http://www.******.com.cn:9099/Service.asmx/getCity?CountryID=1').then((response)=>{
           console.log(response);
       })  
       
       这种写法需要替换成
       axios.get('/apis/Service.asmx/getCity?CountryID=1').then((response)=>{
           console.log(response);
       }) 

##此时，就无需后天做任何的操作，就能跨域请求到，注意：代码必须部署在nginx服务器下面         
            
