# Blog 搭建 #

## 1.购买域名和空间（虚拟主机/VPS/服务器） ##

- 域名： 推荐 Go Daddy 等国外的注册商，方便迁移

- 空间：  
	- 免费空间：如 Github Pages、GitCafe

	- 收费空间：如 阿里云、主机公园等

- 小技巧：
	- 如何查看网站主机空间：
	
			1. http://ip.chinaz.com
			2. http://toolbar.netcraft.com/site_report 

## 2.域名和虚拟主机进行绑定 ##
	
- 域名解析： 
	- https://www.dnspod.cn/console/dns	

	- 阿里云： 万网控制台->云解析DNS
	
			ps：A记录需要添加两条，一条主机记录为 www ，一条为 @ 
			（主要为了不用输入www，直接键入code1024.win即可）
	
- 老薛空间 cPanel 使用：

		1.查看IP： 点击“服务器信息” --> 共享IP地址

## 3.安装博客程序框架 ##
- 选择 Hexo 框架【使用较为主流的 Hexo 静态博客搭建框架（其他框架 Jekyll、wordpress）】
- 搭建流程：
	1. 安装 Git 和 Node.js
	2. 利用 npm(node package manager) 命令安装 Hexo 
		
		`npm install -g hexo`

		ps：自动安装目录 C:\Users\lixianjie\AppData\Roaming\npm

	3. 在你喜欢的磁盘下创建一个 `hexo` 的文件夹，例如 D:\hexo, 并在该文件夹内鼠标右键，然后选择 `git bash here`，输入命令 `hexo init` 回车后耐心等待初始化完成即可
	4. 安装依赖包：输入命令 `npm install` 回车,至此，本地hexo博客已经搭建好了。
- 本地查看 ： 

	1. hexo generate
	2. hexo server (开启服务) 
	3. 在浏览器输入 http://localhost:4000 即可查看
		
## 4.部署到 github ##
   老用户可省略 1、2步骤

  1. 注册 github 账号并创建新的 repository
  2. 设置 ssh 
	  1. 任意位置右键鼠标选择 Git Bash Here ，输入 `ls -al ~/.ssh` ，检查是否存在SSH keys，如存在，删除即可
	  2. `ssh-keygen -t rsa -C "lixianjie——0604@163.com"`
	  	
		(邮箱为注册时的邮箱)然后回车会提示输入passphrase，直接回车3次即可
	  3. `ssh-agent -s`
	  4. `ssh-add ~/.ssh/id_rsa`
	  	
		- 如遇到错误 `Could not open a connection to your authentication agent`

				输入 
					eval `ssh-agent -s`
					ssh-add
				即可

	  5. 到 C:/Users/用户名/.ssh 文件夹下用文本打开 id_rsa.pub 文件复制全部文本内容
	  6. 到自己的 `github` 的 `settings` 里面，在左侧栏内找到 `SSH and GPG keys`,点击 `New SSH key`，把复制的内容粘贴到 key 里面，点击 `Add SSH key` 即可
	7. 测试：ssh -T git@github.com
  3. 在 hexo 文件夹下找到 _config.yml 文件用文本打开修改内容如下：
  
		  deploy:
			  type: git （ps：老版本的这里为 github）
			  repository: git@github.com:你的用户名/你的用户名.github.com.git
			  branch: master
	>（在文本最后可以找到，这里需要注意的是冒号之后需要输入一个空格，否则会报错）
  4. 生成并部署
	
		hexo generate 
		hexo deploy

	>deploy有时会过程缓慢，耐心等待

	 - 问题ps：
        1. hexo deploy 遇到 ERROR Deployer not found: github
	        
		  	>解决办法：`npm install hexo-deployer-git --save` 并将 deploy 的 type由github改为git ，然后重新部署
	    2. hexo deploy 无响应
            >解决办法：检查 _config.yml 文件中的 deploy 下的type等配置，冒号后面要一个半角空格，加上即可
## 5.搭建完成，测试 ##

## 6.将个人域名与 Github Pages空间绑定 ##
- 方法1: 
	 
	在Repository的根目录下面，新建一个名为CNAME的文本文件，
	里面写入你要绑定的域名，不需要添加http/www等前缀。
	但是这样有个弊端就是 deploy 时，repository 根目录下的 CNAME 会自动删除，建议选用 方法2.

- 方法2: 

	在本地hexo/source 文件夹里新建 CNAME 文件，内容为你要绑定的域名，然后重新 generate 和 deploy 即可。

## 其他教程： ##
	
1. [史上最详细“截图”搭建Hexo博客并部署到Github](http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html)

## 其他问题 ##
1. 如何正确上传 README.md 文件到 repository 上？

	如果在线生成 README.md 文件，当 deploy 时，会被删除掉，
	所以正确的方法应该是把 README.md 文件放在 hexo/source 文件夹下，
	然后打开根目录下的 _config.yml 文件，在 skip_render 后面加上 README.md ,
	这样 generate 时就不会解析该文件了，最后再 deploy 即可。
	(因为在 generate 时 ，hexo 会将后缀名为 md 的文件渲染解析成 html 文件)
