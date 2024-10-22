---
layout: post
title: Anaconda常用命令
date: 2024-07-23 00:17:43
description:
tags: Conda Python
categories: Guides
tabs: true
---

## 参考资料

[【精选】Anaconda conda常用命令：从入门到精通_conda命令_笨牛慢耕的博客-CSDN博客](https://blog.csdn.net/chenxy_bwave/article/details/119996001)

## 管理conda自身

### 查看conda版本

```bash
conda --version
```

### 查看conda的[环境配置](https://so.csdn.net/so/search?q=环境配置&spm=1001.2101.3001.7020)

```bash
conda config --show
```

### 设置镜像

conda有时候安装软件会非常慢。设置国内镜像的话可以使安装更快捷一些。设置方法如下所示：

```bash
#设置清华镜像
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
#设置bioconda
conda config --add channels bioconda
conda config --add channels conda-forge
#设置搜索时显示通道地址
conda config --set show_channel_urls yes
```

#### 临时使用

```bash
conda install -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ example_package
```



### 更新conda

推荐总是更新conda

```bash
conda update conda
```

### 更新Anaconda整体

将整个Anaconda都更新到确保稳定性和兼容性的最新版本

```bash
conda update Anaconda
```

### 查询某个命令的帮助 

```bash
conda create --help
```

## 管理环境

conda允许你创建相互隔离的独立环境，这些环境被称之为虚拟环境（Virtual Environment），这些环境各自包含属于自己的文件、包以及他们的依存关系，并且不会相互干扰。

Anaconda有一个缺省的名为base的环境。但是不建议把程序放在base环境中，应该创建不同的虚拟环境分别管理不同的开发项目。这个涉及到一个根本的问题：为什么我们需要虚拟环境呢？举一个简单的例子，想象一下你有多个项目要开发，每个项目中都有一些包要依赖于某个共同的包，但是各自的所需要的版本不一致，有一些需要低版本的，有些需要高版本的。然后你就陷入了众口难调的困境。为不同的项目创建虚拟环境就可以把不同项目隔离开来，各自使用自己所需要的软件环境。

### 创建虚拟环境

使用conda创建虚拟环境的命令格式为:

```bash
conda create -n env_name python=3.8
```

这表示创建python版本为3.8、名字为env_name的虚拟环境。

创建后，env_name文件可以在Anaconda安装目录envs文件下找到。在不指定python版本时，自动创建基于最新python版本的虚拟环境.    

创建虚拟环境的同时安装必要的包(但是并不建议这样做，简化每一条命令的任务在绝大多数时候都是明智的，一个例外是需要反复执行的脚本)

```bash
conda create -n env_name numpy matplotlib python=3.8
```

### 查看有哪些虚拟环境

以下三条命令都可以。注意最后一个是”--”，而不是“-”.

```bash
conda env list
conda info -e
conda info --envs
```

### 激活虚拟环境

使用如下命令即可激活创建的虚拟环境。

```bash
conda activate env_name
```

### 退出虚拟环境

使用如下命令即可退出当前工作的虚拟环境。

```bash
conda activate
conda deactivate
```

### 删除虚拟环境

执行以下命令可以将该指定虚拟环境及其中所安装的包都删除。

```bash
conda remove --name env_name --all
```

如果只删除虚拟环境中的某个或者某些包则是：

```bash
conda remove --name env_name  package_name
```

### 克隆虚拟环境

根据已有环境名复制生成新的环境
假设已有环境名为A，需要生成的环境名为B：

```bash
conda create -n B --clone A
```

根据已有环境路径复制生成新的环境
假设已有环境路径为D:\A，需要生成的新的环境名为B：

```bash
conda create -n B --clone D:\A
```

### 导出环境 

很多的软件依赖特定的环境，我们可以导出环境，这样方便自己在需要时恢复环境，也可以提供给别人用于创建完全相同的环境。

```bash
#获得环境中的所有配置
conda env export --name myenv > myenv.yml
#重新还原环境
conda env create -f  myenv.yml
```

## 包(Package)的管理

### 查询包的安装情况

查询看当前环境中安装了哪些包

```bash
conda list
```

 查询当前Anaconda repository中是否有你想要安装的包

```bash
conda search package_name
```

##  查询是否有安装某个包

用conda list后跟package名来查找某个指定的包是否已安装，而且支持*通配模糊查找。

```bash
conda list pkgname        
conda list pkgname*
```

### 包的安装和更新

在当前（虚拟）环境中安装一个包：

```bash
conda install package_name
```

也可以以以下命令安装某个特定版本的包(以下例为安装0.20.3版本的numpy)：

```bash
conda install numpy=0.20.3
```

 安装包的时候可以指定从哪个channel进行安装，比如说，以下命令表示不是从缺省通道，而是从conda_forge安装某个包。

```bash
conda install pkg_name -c conda_forge
```

### conda卸载包

```bash
conda uninstall package_name
```

这样会将依赖于这个包的所有其它包也同时卸载。

如果不想删除依赖其当前要删除的包的其他包(不建议)：

```bash
conda uninstall package_name --force
```

### 清理anaconda缓存

```bash
conda clean -p      # 删除没有用的包 --packages
conda clean -t      # 删除tar打包 --tarballs
conda clean -y -all # 删除所有的安装包及cache(索引缓存、锁定文件、未使用过的包和tar包)
```

关于清除命令的更详细的说明，可以执行以下命令进行查询：

```bash
conda clean -h
```

## Python版本的管理

除了上面在创建虚环境时可以指定python版本外，Anaconda基环境的python版本也可以根据需要进行更改。

### 将版本变更到指定版本

```bash
conda install python=3.5
```

更新完后可以用以下命令查看变更是否符合预期。

```bash
python --version
```

### 将python版本更新到最新版本

如果你想将python版本更新到最新版本，可以使用以下命令：

```sql
conda update python
```