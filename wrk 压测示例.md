## 压测 lua 脚本

##### 测试环境

**helloworld**

```shell
curl -X "POST" "http://telephone-server-test.lixiaoyun.com/api_data/v1/helloWorld" -d '{"tels":["13034027005"],"days":3}' -H 'Content-Type: application/json' -H 'DATE: 2022-05-16 17:40:033' -H 'SIGN: LXY-V1:JQR361b41e990:f4d458f6596a380f434f4a68138bcc9918014c0f' -H 'INTERVAL: 300000000' -H 'appId: JQR361b41e990'
```

```shell
	wrk -t12 -c1000 -d30s --latency -s post_hello.lua http://telephone-server-test.lxcloud/api_data/v1/helloWorld
```



post_hello.lua

```lua
wrk.method = "POST"
wrk.body = '{"tels":["13034027005"],"days":3}'
wrk.headers["Content-Type"]="application/json"
wrk.headers["appId"]="JQR361b41e990"
wrk.headers["DATE"]="2022-05-16 17:40:033"
wrk.headers["SIGN"]="LXY-V1:JQR361b41e990:f4d458f6596a380f434f4a68138bcc9918014c0f"
wrk.headers["INTERVAL"]="300000000"
function request()
  return wrk.format("POST",nil,nil,body)
end
```



**forbidInfo**

```shell
curl -X "POST" "http://telephone-server-test.lixiaoyun.com/api_data/v1/forbidden_info" -d '{"tels":["13034027005"],"days":3}' -H 'Content-Type: application/json' -H 'DATE: 2022-05-16 17:18:052' -H 'SIGN: LXY-V1:JQR361b41e990:538322cc3bcc82692e3f5fd2e2e853f2836b6457' -H 'INTERVAL: 300000000' -H 'appId: JQR361b41e990'
```



```shell
	wrk -t12 -c1000 -d30s --latency -s post_forbid.lua http://telephone-server-test.lxcloud/api_data/v1/forbidden_info
```



post_forbid.lua

```lua
wrk.method = "POST"
wrk.body = '{"tels":["13517241266"],"days":3}'
wrk.headers["Content-Type"]="application/json"
wrk.headers["appId"]="JQR361b41e990"
wrk.headers["DATE"]="2022-05-17 22:31:029"
wrk.headers["SIGN"]="LXY-V1:JQR361b41e990:395f17654f0c481be38c2150f8222c43042f3708"
wrk.headers["INTERVAL"]="300000000"
function request()
  return wrk.format("POST",nil,nil,body)
end
```



### 20200516 测试环境压测

![image-20220516174845042](http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220516174845042.png)

![image-20220516174943737](http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220516174943737.png)



## 20200517 生产环境压测

![image-20220517202718779](http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220517202718779.png)

![wecom-temp-1897b1b9a7f9945622eec2323d8f2233](http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/wecom-temp-1897b1b9a7f9945622eec2323d8f2233.png)

20m带宽 8c16G * 5 / 8C16G * 7 

![image-20220517230438994](/Users/shifty/Library/Application%20Support/typora-user-images/image-20220517230438994.png)

![image-20220518104258639](http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220518104258639.png)



