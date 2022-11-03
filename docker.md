# 

Docker

# 

docker的使用及常用指令(Debian系统)

## 

1.docker是什么？

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/uGvtRXLVnDXPBHIaZr84_sID3AoHqGZdm2z57Eo0KtE.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

[Docker快速入门之原理篇 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/31654581)

## 

2.安装vmware,debian系统

[资源中心 | 统信UOS生态社区 (chinauos.com)](https://www.chinauos.com/resource/download-home)

debian系统 iso文件

## 

3.UOS系统配置

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/vgZN5HegnM3viaGzyuiwpIVqlbFfkxjRs-bcQOfGFgk.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

## 

4.docker安装

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/eMNatonUYUR9W0Zxk-nh9iX3RO-7PEFI6pJXvQG0YVg.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

1. sudo 

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/tVST33G6kZZ2Kfk5Zcqw_pQ0MpwbAWPLa3HcjDEnhfc.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

2. apt-get

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/7eK_zck8dnvYoctYf9urQr3ywx5irXM01pL9O56FGEc.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

[(19条消息) apt-get命令详解（超详细）_迎面暖风的博客-CSDN博客_apt-get](https://blog.csdn.net/Netfilter007/article/details/103879962)

3. touch

创建文件和修改文件或者目录的时间戳

4. vim 

vim是全屏幕纯文本编辑器

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/SwF2j1pPpmsJurRTBiy1hO2I-ffD5PHsp5TKqv___nU.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/Thq4rs8IgaKFKv3fHDlQqvbW0OAYQj4-2cLmo6JtWI8.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

首先进入命令模式，输入i进入输入模式，ESC退出输入模式。

- w保存文件

- q退出文件

[Linux vi/vim | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-vim.html)

## 

5.Docker配置生效

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/54n3KxDzNajAtwX3tdyevUwcZQ4jynjZviRP-xi_RdA.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/SLX1q2xl0THOq-9NU_ieCQOe2M1OI4fkhRHhkw1ayTE.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

##### 

apt-mark：锁定docker版本取消自动安装

## 

6.docker常用命令

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/xZEhsxrFHVts6R-_YRRhtX-zbxPDrxZFmXvqR-lWbvM.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

## 

7.Docker仓库搭建

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/5i6WeDUD-RcyIhZYfWD2NB5qLrIKDaMXExGRZBAF104.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

## 

8.前端配置 --nginx

下载最新版本Nginx的镜像

docker pull nginx:latest

查看镜像库

docker image

加载映射文件

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/F9nXucv_qo10pU40Orl7_WOlERtshDN4879-MCEJFCE.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)

运行nginx镜像

docker run -id -p 8090:80 -p 8091:443 --name=LBWnginx -v /opt/nginx/html:/usr/share/nginx/html -v /opt/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /opt/nginx/logs:/var/log/nginx -v /opt/nginx/conf/conf.d:/etc/nginx/conf.d -v /opt/nginx/ssl:/etc/nginx/ssl nginx

docker ps 可以看到当前运行的容器

docker ps -a 可以看到所有容器

nginxconf

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/7f342c26-f3c1-4461-87e2-015ea43adbbc/resources/j5SxNT1z_XYZtMpBclL58pnoIPqUJtzpvZbUOteW1R8.png?token=W.8sOFtUO08UC3QxuyNQypZbRG1BHX43FFLbdc9q4EmGkfSLxlZ7-JxFVuZt5Q8cI)
