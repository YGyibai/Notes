**root** **与** **alias**

假如服务器路径为：/home/imooc/files/img/face.png

root 路径完全匹配访问

配置的时候为：

```apl
location /imooc { 

		root /home 

}
```

用户访问的时候请求为： url:port/imooc/files/img/face.png

alias 可以为你的路径做一个别名，对用户透明

配置的时候为：

```apl
location /hello { 

			root /home/imooc 

}
```

用户访问的时候请求为： url:port/hello/files/img/face.png ，如此相当于为目录 imooc 做一个自定义的别名。