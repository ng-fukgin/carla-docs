更新CARLA
=======

要发布问题、疑问或建议，请随时登录 CARLA 官方中文论坛！

* * *

更新命令摘要
------

    # Update a CARLA packaged release. 
    #   1. Delete the current one. 
    #   2. Follow the Quick start installation to get the one desired. 
    
    
    # Update Linux build. 
    git checkout master
    make clean
    git pull origin master
    ./Update.sh
    
    
    # Update Windows build. 
    git checkout master
    make clean
    git pull origin master
    #   Erase the content in `Unreal\CarlaUE4\Content\Carla`.
    #   Go to `\Util\ContentVersions.txt`.
    #   Download the latest content.
    #   Extract the new content in `Unreal\CarlaUE4\Content\Carla`. 
    
    
    # Get development assets. 
    #   Delete the `/Carla` folder containing previous assets. 
    #   Go to the main carla folder.
    git clone https://bitbucket.org/carla-simulator/carla-content Unreal/CarlaUE4/Content/Carla

获取最新的二进制版本
----------

二进制版本是预先打包的，因此与特定版本的 CARLA 相关联。 要获取最新版本，请删除以前的版本并按照快速启动安装获取所需的版本。版本列在 CARLA 存储库的开发中。 还有一个高度实验性的 Nightly 构建，包含最新的 CARLA 状态。

 [Get releases](https://github.com/carla-simulator/carla/blob/master/Docs/download.md "Go to the list of CARLA releases")

 [Get the nightly build](http://carla-releases.s3.amazonaws.com/Linux/Dev/CARLA_Latest.tar.gz "Go to the nightly CARLA build")

* * *

更新 Linux 和 Windows 版本
---------------------

确保在更新前位于本地主分支。 然后，将更改合并或变基到其他分支并解决可能的冲突。

    git checkout master
    

### 清理构建

转到主 CARLA 文件夹并删除先前构建生成的二进制文件和临时文件。

    make clean
    

### Pull from origin

在Carla资源库中获取现有版本 `master` 

    git pull origin master
    

### 下载资产素材

**Linux.**

    ./Update.sh
    

**Windows.**

**1.** 在 `Unreal\CarlaUE4\Content\Carla`中抹去之前的版本  
**2.** 访问 `\Util\ContentVersions.txt`.  
**3.** 为 `latest`下载内容  
**4.** 从 `Unreal\CarlaUE4\Content\Carla`中提取新内容

Note

为了配合 CARLA 团队正在开发的内容，请前往下方获取开发资产。

### 登录服务器

在spectator view 中运行服务器以确保一切正常

    make launch
    

* * *

获取开发资产
------

CARLA 团队使用仍在开发中的资产。 这些模型和地图有一个公共 git 存储库，CARLA 团队定期在其中推送最新更新。 资产仍未完成，仅建议开发人员使用它们。

[public git repository](https://bitbucket.org/carla-simulator/carla-content) 

为了处理这个存储库，建议安装 git-lfs。 存储库会定期修改，并且 git-lfs 可以更快地处理大型二进制文件。

[git-lfs](https://github.com/git-lfs/git-lfs/wiki/Installation). 

要克隆存储库，请转到主 CARLA 目录并运行以下命令。

To clone the repository, **go to the main CARLA directory** and run the following command.

    git clone https://bitbucket.org/carla-simulator/carla-content Unreal/CarlaUE4/Content/Carla
    

Warning

在克隆存储库之前删除包含资产的 /Carla 文件夹。 否则会显示错误。



