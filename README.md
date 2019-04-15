# ELKRedisLogService
分布式日志系统：ELK+Redis+Nginx

ELK是一个成熟的日志系统，主要功能有收集、分析、检索,加上Redis作为日志采集口，业务量大可采取Ngninx进行负载均衡。
一：ELK在Lunux(	CentOS7)上的操作安装：
   操作系统及各组件版本
	centos-7-x86_64
	java8
	elasticsearch-6.2.4
	kibana-6.2.4
	logstash-6.2.4
	
	防火墙禁用：[root@localhost ~]# vim /etc/sysconfig/selinux
   1.JAVA
     下载：[root@localhost ~]# wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http:%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" 
	                     "https://download.oracle.com/otn-pub/java/jdk/8u201-b09/42970487e3af4f5aa5bca3f542482c60/jdk-8u201-linux-x64.rpm"
     安装：[root@localhost ~]# rpm -ivh jdk-8u201-linux-x64.rpm
	 检查JAVA是否安装成功：[root@localhost ~]# java -version
                            java version "1.8.0_171"
	
   2.Elasticsearch(默认9200端口)
     下载：[root@localhost ~]# wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.4.rpm   
	 安装：[root@localhost ~]# rpm -ivh elasticsearch-6.2.4.rpm
	 配置：[root@localhost ~]# vim /etc/elasticsearch/elasticsearch.yml
	        取消43、59行的注释，行号不一定准确，但一定是下面这几项
				bootstrap.memory_lock: true
				http.port: 9200
	 启动：	[root@localhost ~]# systemctl daemon-reload
			[root@localhost ~]# systemctl enable elasticsearch
			[root@localhost ~]# systemctl start elasticsearch
			[root@localhost ~]# netstat -plntu
   【注：最后通过netstat -plntu查看es是否启动成功，如果有端口号为9200的输出那就代表es启动成功了】					
	3.Kibana(默认5601端口)
     下载： [root@localhost ~]# wget https://artifacts.elastic.co/downloads/kibana/kibana-6.2.4-x86_64.rpm
     安装：	[root@localhost ~]# rpm -ivh kibana-6.2.4-x86_64.rpm
	 配置： [root@localhost ~]# vim /etc/kibana/kibana.yml
	         取消2、7、21行的注释，行号不一定准确，但一定是下面这几项
				server.port: 5601
				server.host: "localhost"
				elasticsearch.url: "http://localhost:9200"
	 启动：	[root@localhost ~]# systemctl enable kibana
			[root@localhost ~]# systemctl start kibana
			[root@localhost ~]# netstat -plntu	
    【注：和elasticsearch一样，最后通过netstat -plntu查看kibana是否启动成功，如果有端口号为5601的输出那就代表kibana启动成功了】		
     4.Logstash
	 下载：[root@localhost ~]# wget https://artifacts.elastic.co/downloads/logstash/logstash-6.2.4.rpm
	 安装：[root@localhost ~]# rpm -ivh logstash-6.2.4.rpm
	 启动：[root@localhost ~]# systemctl enable logstash
           [root@localhost ~]# systemctl start logstash