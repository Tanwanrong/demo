# 从gitlab到github的迁移

## 1.GitHub上用来测试的项目地址：https://github.com/intel-sandbox/dsg-bmc_dsg-openbmc-meta-intel

## 2.先将gitlab上的文件git clone到本地，再push到github

## 3.在github上建了一个放jenkinsfile的repo, 命名为DSG-Openbmc-Jenkins，建立好repo后会自动生成从本地push 项目的指令



##4.github和gitlab都是通过webhook进行双向通信，

## 5.迁移需要改动的地方：

4.1   jenkins中项目仓库的git地址

![image-20210709210637067](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210709210637067.png)

4.2  设置 jenkins中的webhook

点击manage Jenkins-- configure system

![image-20210709213358774](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210709213358774.png)



在界面上按ctrl +F搜索github，找到Jenkins中对应github的webhook的位置

![image-20210709211705559](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210709211705559.png)

payload URL中填写对应的webhook地址，前面需要加一个网址，在1source中有写

![image-20210709214022413](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210709214022413.png)

<img src="C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210709213752286.png" alt="image-20210709213752286" style="zoom:67%;" />



4.3  jenkins中具体的pipeline文件需要修改

# 6.github在使用git push推代码时候需要

##6.1先设置代理（可能因为连接的是公司内部的网络）

git config --global http.proxy http://child-prc.intel.com:913

git config --global https.proxy http://child-prc.intel.com:913

## 6.2 获取个人token，github采用账号+token 密码的形式推代码

### 点击个人账户的setting界面可以获取token

![image-20210709212810812](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210709212810812.png)

## 7.gitlab中的master分支在github中叫做main



# 8.权限管理

![image-20210709215516961](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210709215516961.png)

# 9. git add ./

在$ git commit -m "first commit"之前需要用 git add./

# 10.在Jenkins上面新建仓库在github上面的trigger job

1. 新建job ，job类型仍然选择multipipeline
2. 创建好job后进行设置，填入repo 的git地址，选择证书

* <img src="C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210714141023081.png" alt="image-20210714141023081" style="zoom: 50%;" />
  * 证书的设置如下:

<img src="C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210714134858344.png" alt="image-20210714134858344" style="zoom:60%;" />

<img src="C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210714135712185.png" alt="image-20210714135712185" style="zoom: 67%;" />

![image-20210714140046573](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210714140046573.png)



证书的形式有很多种，这里选择了用户密码的形式

![image-20210714140139853](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210714140139853.png)

+ pipeline中修改environment的build path为对应的值

environment {
        BuildJobPath = 'DSGBMC_1source/dsg-openbmc-ci/main'
    }

![image-20210714143612248](C:\Users\wanrongt\AppData\Roaming\Typora\typora-user-images\image-20210714143612248.png)


