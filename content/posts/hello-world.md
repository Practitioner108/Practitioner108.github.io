---
title: "博客部署记录"
draft: false
---

#从零开始：在Ubuntu20.04上部署个人博客教程（Hugo + GitHub Pages部署实践）

##目标
1. 建立一个个人技术博客，用于记录机械臂路径规划学习过程中的工程实践和学术理论。
2. 环境依赖
    1. Ubuntu20.04 LTS
    2. Hugo Extended
    3. PaperMod主题
    4. VScode

##1 基础搭建与Git初始化
1. 环境准备
    1. 安装hugo
        首先需要安装hugo，hugo可以看作是一个“转换器”，可以把Markdown文档、模板等素材变成一个完整的静态网页，是一个静态网站生成器。
        但是要注意hugo本身的版本要和系统版本相匹配，可以使用
        - [hugo下载链接](https://gh-proxy.com/https://github.com/gohugoio/hugo/releases/download/v0.110.0/hugo_extended_0.110.0_linux-amd64.deb)
        该链接版本与Ubuntu20.04相匹配，可以直接使用。

        在下载完毕后进入Downloads文件夹
        '''
        cd ~/Downloads
        '''
        然后安装下载好的包
        '''
        sudo dpkg -i hugo hugo_extended_0.110.0_linux-amd64.deb
        '''
        最后验证一下
        '''
        hugo version
        '''
        如果正常显示版本号就说明成功了


    2. 创建博客“骨架”
        首先需要在任意目录下（我是在home下面直接建立）建立：
        '''
        hugo new site my_robotics_blog
        cd my_robotics_blog
        '''
        其中my_robotics_blog是自己给自己空间起的名字
        然后初始化Git仓库
        '''
        git init
        '''


    3. 下载主题
        关于主题也要注意，同样要与版本相匹配，如果过新或者过旧都会导致不匹配而无法使用。
        我这里PaperMod主题拿举例
        '''
        git clone https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
        '''
        下载好之后还需要将主题代码回滚到2023年4月1日之前的状态，因为那个时候Hugo v0.110.0 还是主流，肯定能兼容.
        先去到主题的文件夹
        '''
        cd ~/github_projects/my_robotics_blog/themes/PaperMod
        '''
        然后穿越时间
        '''
        git checkout $(git rev-list -n 1 --before="2023-04-01" HEAD)
        '''
        这条命令的意思是：找到2023-04-01之前的最后一个版本，并切换过去

        然后回到博客的根目录
        '''
        cd ../..
        hugo server -D
        '''
        这个时候就会出现:
        '''
                Start building sites … 
        hugo v0.110.0-e32a493b7826d02763c3b79623952e625402b168+extended linux/amd64 BuildDate=2023-01-17T12:16:09Z VendorInfo=gohugoio

                        | EN  
        -------------------+-----
        Pages            | 13  
        Paginator pages  |  0  
        Non-page files   |  0  
        Static files     |  0  
        Processed images |  0  
        Aliases          |  3  
        Sitemaps         |  1  
        Cleaned          |  0  

        Built in 47 ms
        Watching for changes in /home/tu/github_projects/my_robotics_blog/{archetypes,content,data,layouts,static,themes}
        Watching for config changes in /home/tu/github_projects/my_robotics_blog/config.toml
        Environment: "development"
        Serving pages from memory
        Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
        Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
        Press Ctrl+C to stop

        Change detected, rebuilding site.
        2025-12-05 14:52:32.377 +0800
        Source changed WRITE         "/home/tu/github_projects/my_robotics_blog/content/posts/hello-world.md"
        Total in 6 ms

        Change detected, rebuilding site.
        2025-12-05 15:41:48.878 +0800
        Source changed WRITE         "/home/tu/github_projects/my_robotics_blog/content/posts/image.png"
        Total in 8 ms

        Change detected, rebuilding site.
        2025-12-05 15:43:11.378 +0800
        Source changed RENAME        "/home/tu/github_projects/my_robotics_blog/content/posts/image.png"
        Total in 6 ms
        '''
        这样的提示，说明启动成功了。
2. 操作指南
    1. 使用步骤
        1. 保持服务器开启（就是那个hugo server -D）
        2. 另外新开一个终端，用自己喜欢的方式编辑文件，我这里使用的是VScode，直接“code xxx.md”就行
    2. VScode
        一开始检测到你开启了md文件，code一定会检测出来，并且右下角一定会蹦出一个提示“Git repositories were found in the parent folders of the workspace or the open file(s). Would you like to open the repositories?”，直接Yes就行。
        但是还有更便捷的办法，比如直接打开文件夹：
        '''
        code ~/github_projects/my_robotics_blog
        '''
        然后你就会发现左边有一个侧边栏看着就很帅的面板，这是VScode的“源代码管理”（Source Control）面板。
        
        *如何使用该面板*
        1. 怎么分区的？
            - Staged Changes（暂存的更改）
                代表里面的文件已经准备好，随时可以提交。
            - Changes（更改）
                代表里面的文件你刚修改或新建，Git还不知道要不要保存。
        2. 如何提交保存？
            1. 第一步，将鼠标悬停在Changes上，点击右边的“+”，相当于告诉Git，这些文件我要保存
            2. 第二步，在最上面“Commit”按键的上面写上这次的改动说明。
            3. 第三步，点击“Commit”提交
                【注意】：在第一次提交的时候会弹出窗口，这是Git在向作者要名片，这个时候：
                1. 关闭窗口
                2. 唤起VScode下方终端（使用Ctrl + ~唤起）
                3. 设置用户名（可以用Github的昵称或者自己的名字）
                    '''
                    git config --global user.name "你的GitHub昵称"
                    '''
                4. 设置邮箱（注册Github的邮箱）
                    '''
                    git config --global user.email "你的邮箱@example.com"
                    '''
                搞定之后，重新提交
        3. 文件右边的字母分别是什么意思？
            | 符号 | 含义（中文） | Git 命令状态 | 说明 |
            |:---:|:---:|:---:|:---|
            | U | Untracked（未追踪） | ?? | 文件是新的，Git 还没开始管它（比如 hello-world.md）。需要 git add。 |
            | A | Added（已添加） | A | 文件已被你用 + 号（即 git add）标记，准备提交。 |
            | M | Modified（已修改） | M | 文件已被修改，但还没暂存。 |
            | D | Deleted（已删除） | D | 文件已从本地删除。 |
            | R | Renamed（已重命名） | R | 文件名或路径已更改。 |

    3. 文件本身的相关问题
        1. 在文件开头一般会有“draft: false”这样的字样，如果是false，那就说明是正式文章，Hugo会把它编译进网页，如果是true则会认定是草稿。
    4. Github相关问题
        1. 如何上传到自己Github的账户上？
