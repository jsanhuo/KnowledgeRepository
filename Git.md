[TOC]

# Git

## Git简介

### 1.git的优势

1. 大部分在本地完成，不需要联网
2. 完整性保证
3. 尽可能添加数据而不是删除或修改数据
4. 分支操作非常快捷流畅
5. 与Linux命令全面兼容

### 2.git结构

![image-20211018123303102](assets/image-20211018123303102.png)

本地库通过`git push`提交到远程库。

远程库到本地库通过`git clone`下载

远程库到远程库复制`git fork`

![image-20211018124216921](assets/image-20211018124216921.png)



## Git命令

### 1.本地库初始化

- 命令

```shell
git init
```

- 效果

![image-20211018124813073](assets/image-20211018124813073.png)

- 注意

  运行后生成.git文件，.git文件存放的是本地库相关的子目录和文件不能删除和修改。

### 2.设置签名

- 形式

  用户名：

  Email：

- 作用

  区分不同的开发人员身份

- 辨析

  与远程库的账号密码没有关系

- 命令

  - 项目级别/仓库级别：尽在当前本地库有效

    ```shell
    git config user.name xxx
    git config user.email xxxx@xx.com
    ```

    信息保存位置为.git目录下config文件

    ![image-20211018130035258](assets/image-20211018130035258.png)

  - 系统用户级别：大恩如当前操作系统的用户范围

    ```shell
    git config --global user.name xxx
    git config --global user.email xxxx@xx.com
    ```

    信息保存位置：~/.gitconfig文件

    ![image-20211018130303968](assets/image-20211018130303968.png)

  - 级别优先级

    就近原则：项目级别优先于系统用户级别

    如果只有系统用户级别的签名，就以系统用户级别签名为准

    二者都没有不允许

  





