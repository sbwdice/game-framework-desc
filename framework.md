##moblie game framework

1.获取服务器列表流程  

      请求服列表信息					交给cgi处理请求                      返回服务器列表  
		 
  client------------------>nginx server/apache server--------------------->cgi/fast-cgi------------------------->client  
  
2.登录操作  

                登录请求                        数据库验证                用户信息/角色信息    
		  
	client------------------>login server --------------->dataaccess-------------------->login server   
	
	返回用户信息/角色信息(包含gameworld ip端口信息)        根据下发信息登录到对应的gameworld   
	
	------------------------------------->client  -------------------------------->gameworld  
	
3.windows 环境搭建整套系统  

	3.1安装nginx,  配置conf/nginx.conf  方便区分多个server,使用include 文件夹/*.conf; 会使用文件夹内所.conf  
		几个常用的nginx命令:start nginx; nginx -s stop; nginx -s reload(重读配置)  
		配置例子：
>		server
		{
			listen       端口;
			server_name  ip;
			root D:/game_htdocs（网站目录/php目录）;
			index index.html index.htm index.php;  
			
			location ~ .*\.(php|php5)?$		//所有php请求交给 127.0.0.1:9000 的fastcgi处理  
			{
				include fastcgi.conf;
				fastcgi_pass 127.0.0.1:9000;
				fastcgi_index index.php;
			}
		
			location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
			{
				expires      30d;
			}
		
			location ~ .*\.(js|css)?$
			{
				expires      1h;
			}
	
			location / {
				try_files $uri $uri/ /$uri.php$is_args$args;
			}
		
			access_log off;
		}

	3.2安装php-cgi下载安装php，将php.ini.development或者php.ini-production改成php.ini  
	
	将error_reporting = E_ALL注释， 改为error_reporting = E_ERROR（减少错误输出），其他配置优化请熟悉再配置
	启用php-cgi.exe 命令：  php-cgi.exe -b 127.0.0.1:9000 -c php.ini（cmd模式下，启动后不可关闭cmd窗口，关闭了php-cgi也关闭了）  
	
	3.3安装mysql配置游戏数据库，配置相关端口连接配置等等  
	
	4.4把gateway login dataccess gameworld 四个进程启动，整个服务器架设完毕
	
