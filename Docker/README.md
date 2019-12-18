[TOC]



# Docker

## Docker简介

* **Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。**它是目前最流行的 Linux 容器解决方案。
* Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。
* 总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。

## Docker安装教程

* 网上教程随便一搜就有，这里我就只进行Ubuntu18.04LTS的安装，当然更推荐看[Docker的官方安装文档](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
* 毕竟我现在是用的是Ubuntu18.04LTS的系统，用Windows的也建议装个Linux系统，装双系统的教程之后再转。
* 极其不推荐使用虚拟机，原因就不说了，用过的都知道

* 好了不多说了，接下来上教程

  * OS要求

    * 迪斯科19.04
    * 宇宙18.10
    * 仿生18.04（LTS）
    * Xenial 16.04（LTS）

  * 卸载旧版本

    * ```shell
      $ sudo apt-get remove docker docker-engine docker.io containerd runc
      ```

      如果apt-get报告没有安装这些软件包就可以了

  * 安装Docker Engine - Community

    * 在新主机上首次安装Docker Engine - Community之前，需要设置Docker存储库。之后，您可以从存储库安装和更新Docker。

      * 更新apt包索引

        ```shell
        $ sudo apt-get update
        ```

      * 安装包以允许`apt`通过HTTPS使用存储

        ```shell
        $ sudo apt-get install \
            apt-transport-https \
            ca-certificates \
            curl \
            gnupg-agent \
            software-properties-common
        ```

      * 添加Docker的官方GPG密钥

        ```shell
        $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
        ```

      * 通过搜索指纹的最后8个字符，验证您现在拥有带指纹的密钥

        ```shell
        $ sudo apt-key fingerprint 0EBFCD88
        ```

        你会得到如下的结果

        ```shell
        pub   rsa4096 2019-09-04 [SCEA]
              9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
        uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
        sub   rsa4096 2019-09-04 [S]
        ```

      * 使用以下命令设置**稳定**存储库。要添加 **夜间**或**测试**存储库，请在下面的命令中的单词后添加单词`nightly`或`test`（或两者）`stable`。[了解**夜间**和**测试**频道](https://docs.docker.com/install/)

        ```shell
        $ sudo add-apt-repository \
           "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
           $(lsb_release -cs) \
           stable"
        ```

    * 安装Docker Engine - Community

      * 更新apt包索引

        ```shell
        $ sudo apt-get update
        ```

      * 安装*最新版本*的Docker Engine - Community和 containerd，或者转到下一步安装特定版本

        ```shell
        $ sudo apt-get install docker-ce docker-ce-cli containerd.io
        ```

      * 要安装*特定版本*的Docker Engine - Community，请列出repo中的可用版本，然后选择并安装

        列出您的仓库中可用的版本:

        ```shell
        $ apt-cache madison docker-ce
        ```

        得到如下结果

        ```shell
          docker-ce | 5:18.09.1~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
          docker-ce | 5:18.09.0~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
          docker-ce | 18.06.1~ce~3-0~ubuntu       | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
          docker-ce | 18.06.0~ce~3-0~ubuntu       | https://download.docker.com/linux/ubuntu  xenial/stable amd64 Packages
        ```

        例如，使用第一列中的版本字符串安装特定版本 5:18.09.1~3-0~ubuntu-xenial

        ```shell
        $ sudo apt-get install docker-ce=5:18.09.1~3-0~ubuntu-xenial docker-ce-cli=5:18.09.1~3-0~ubuntu-xenial containerd.io
        ```

      * 通过运行`hello-world` 映像验证是否正确安装了Docker Engine - Community

        ```shell                                                                                                                                                                                         
        $ sudo docker run hello-world
        ```

        此命令下载测试映像并在容器中运行它。当容器运行时，它会打印一条信息性消息并退出

      
      * Docker Engine - 社区已安装并正在运行。该`docker`组已创建，但未向其添加任何用户。您需要使用它`sudo`来运行Docker命令,如果您想将Docker用作非root用户，您现在应该考虑将您的用户添加到“docker”组，例如：
      
        ```shell
          sudo usermod -aG docker your-user
        ```
      
        请记得注销并重新登录才能生效

