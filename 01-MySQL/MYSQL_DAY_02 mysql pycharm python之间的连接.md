MYSQL_Day_02 mysql pycharm python之间的连接

 1打开pycharm 点击新建一个文件

![img](https://i-blog.csdnimg.cn/direct/effe3e39eed2405c9a6b64ed7cf84f47.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑创建一个文件夹 用来放 你未来所有的项目 可以像我一样 all project 是我全部项目的根目录 

里面就是project001 

然后假如你在下面那个箭头 里面 没有看到 自己下载的python版本 此时你需要点击右边的add interpreter

![img](https://i-blog.csdnimg.cn/direct/f002d5b5c5c143e58ed2367bf45ad91e.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

点击第一个

![img](https://i-blog.csdnimg.cn/direct/d19da5affc884b9db031371251154b3d.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

然后在右边 找到你的python 后缀是exe的文件 例如

![img](https://i-blog.csdnimg.cn/direct/bc32dcf30a0945a294675c5d6cf3e5f1.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

我的python文件在

D盘 software 目录 下 的python3143目录 里面的一个libs里面的 python.exe 

点击就会发现上面可以了 此时 你配置好了第一个环境 将会如下面如此    

![img](https://i-blog.csdnimg.cn/direct/c06e66f92b5c4ba49d787bfc4afb2772.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

# 2 现在要开始连接上mysql了

![img](https://i-blog.csdnimg.cn/direct/ec5a3799c41f4b2e97d2b2629c89a3ec.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

找到右边的这个 将他拖拽的左边 我习惯放在一边 

![img](https://i-blog.csdnimg.cn/direct/2b7f1bac3d8e43c284a8f4368a634f93.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这样子 上面就是文件夹 下面就是数据库了

# 现在开始让pycharm连接mysql

![img](https://i-blog.csdnimg.cn/direct/d923bf3e408b400bac6d3e7b2e2bbe65.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

打开之后是下面这样子的
 然后按照下图进行操作哦 每一步都有提醒了![img](https://i-blog.csdnimg.cn/direct/7e03e04db25a42eba7d025e3fd74446d.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑

最下面需要去验证并且下载mysql的jar包 所以需要网络好 假如不好没成功的话 需要去网上找到别人提供好的jar包 手动添加 

成功了就可以直接点右下角的apply 和ok啦
 假如不成功也没关系 

也可以手动添加mysql 跟着下面操作
 ![img](https://i-blog.csdnimg.cn/direct/555428e008da45e0a0e6b97cd0865f73.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑将进入下面这个界面 按照下图 点击 可以手动找到 mysql的jar包
 ![img](https://i-blog.csdnimg.cn/direct/0b73153a67fc430dbf50d32978a932fd.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑![img](https://i-blog.csdnimg.cn/direct/b3542e4c4d9c4698a45294234e8b8fe8.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑

找到这个包之后就可以点击应用了 最后就成功创建好啦
 就会是下面这样子了
 ![img](https://i-blog.csdnimg.cn/direct/6b25a8822a344ac199c7e7794c3ed057.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑

接下来我们可以简单创建一个文件 把他们连接上 因为 mysql的代码 需要在pycharm里面展现出来
 跟着我来![img](https://i-blog.csdnimg.cn/direct/848855a1858a4a91bd87507d065998fc.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑

创建一个后缀为 .sql的文件

![img](https://i-blog.csdnimg.cn/direct/71e6053b9bc94c84b2b3a8317a53a029.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

会出现这个图标
 ![img](https://i-blog.csdnimg.cn/direct/e72057d7de2b42338423146340cac9d7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑

可以简单实验一下 

先教大家一个简单的代码
 show databases;  #查看当前所有的数据库 

切记一定要打 ; 分号哦 然后按快捷键 ctrl+enter 执行光标所在行的代码
 就可以看到这个了
 ![img](https://i-blog.csdnimg.cn/direct/e56c94fa981c4bf68520df75b47e2664.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑
 随便点击一个
 可以看到运行成功了 代码左边会有绿色的√![img](https://i-blog.csdnimg.cn/direct/6c8828a6660a4ff7b503b65c4364d10f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​编辑底下就会出来 当前有的数据库  下一期会教你如何使用mysql
 下期见!!! 
