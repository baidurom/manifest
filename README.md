1. 介绍
====
<b>Coron</b>是一个致力于开源ROM制作的项目, 开源了制作百度云OS 的所有工具和部分示例机型。采用Apache License 2.0协议, 为乐于分享的开发者提供最大的自由度。

开源项目的访问网址是 https://github.com/baidurom , 创立纪元是二零一三年八月八日。旨在让更多的开发者体验百度云OS 的制作过程, 感受其间简洁、细节的情怀。

   <b>Coron</b>, 意味着与开发者合作而生的ROM, <b>CO</b>-operation <b>ROM</b>；

   <b>Coron</b>, 意味着百度云OS 强大的云服务, <b>ROM</b> <b>O</b>ver <b>C</b>loud；

   <b>Coron</b>, 也是一个清新纯净的小岛。


2. 分支命名
===
开源项目的分支命名基于coron, 对于单卡机型, 后缀为Android 版本； 对于双卡机型, 后缀为双卡平台与Andorid版本的结合。

已有单卡分支有coron-4.0, coron-4.1, coron-4.2, coron-4.3, coron-4.4；

已有的双卡分支有coron-mtk-4.0, coron-mtk-4.2。 

分支对应到可以制作的ROM版本, 譬如, 厂商原来的系统是Android 4.4的单卡版本, 那么就推荐使用coron-4.4分支来移植百度云OS 。

开源项目的目录结构如下所示: 

    coron
     +-- manifest                 开源项目的Repo 管理清单文件
     +-- tutorials                开发文档、教程
     +-- build                    编译脚本，包括基于Makfile 编译环境的构建脚本
     +-- tools                    工具，包括反编译/编译，解包/打包的脚本，以及其他一些实用工具
     +-- baidu
          +-- release             Baidu 发布的默认底包，内容定期更新
          +-- frameworks
                +-- overlay       资源覆盖，包括Baidu 对原生Android 资源文件的修改
     +-- devices                  所有的开发机型目录
          +-- base                基础机型，生成其他机型的Patch，内容定期更新
          +-- your_device         待开发者适配的机型



3. 代码下载
====

通过repo init命令的-b参数, 选择需要下载的分支(譬如coron-4.4)。
通过repo sync命令同步远程代码: 

    repo init -u https://github.com/baidurom/manifest.git -b coron-4.4
    repo sync

如果连接一直失败或下载代码过慢，则使用以下命令:

    repo init --repo-url git://github.com/baidurom/repo.git -u https://github.com/baidurom/manifest.git -b coron-4.4 --no-repo-verify
    repo sync --no-clone-bundle -c -j4


4. 百度云OS 移植
===
<b>* 一键适配</b>

下载完代码以后, 在开源项目根目录, 执行以下命令初始化开发环境: 

    source build/envsetup.sh

创建一个新的机型工程的目录(以demo为例), 后续的移植都在机型目录完成。

    mkdir -p devices/demo
    cd devices/demo

通过 USB 线连接 PC 与待开发的手机,或将待移植的原厂底包放置于机型根目录。执行以下命令,便可开始一键适配

    coron fire

一键适配有一些关键的步骤, 该命令会记录当前的执行到的步骤, 如果其中某个步骤执行出错, 只需要根据错误提示解决问题后, 继续执行该命令即可。

    1) config: 从手机或已有的原厂底包中拉取boot.img和recovery.img,生成Makefile;
    2) newproject: 从手机或已有的原厂底包中拉取原厂的所有文件,构建一个新机型工程;
    3) patchall: 自动 Patch 需要植入的代码。既插桩;
    4) autofix: 自动补充Phone， SystemUI等模块中缺失的接口;
    5) fullota: 编译机型,生成最终的卡刷包或可以刷入手机的 image。


<b>* 冲突处理</b>

自动将百度云OS 涉及到的改动注入厂商的代码中, 可能会存在代码合并冲突。冲突会以下面的形式标注出来, 开发者需要在厂商的文件中手工解决这些冲突。

    <<<<<<< VENDOR
      原厂的代码块
    =======
      百度源码的代码块
    >>>>>>> BOSP


<b>* 版本升级</b>

在适配完一款新机型后, 当百度云 OS 有版本更新时, 会发布最新的代码改动, 通过以下命令, 便能自动升级到最新版本

    coron upgrade


5. 交流讨论
===
具体机型一般有特定的问题, 等待开发者去解决, 以下文档可以帮助开发者解决一些实际问题：

《Developer-Guide.pdf》, 《Details-to-Smali-Development.pdf》

欢迎加入一起交流讨论适配中的各种问题，我们会有定期的开发者学院课程辅导。

  BBS    : http://bbs.rom.baidu.com/forum-184-1.html

  QQ     : 385386883


6. 代码提交
===
代码提交有2种方式: 

<b>1) 直接更新Git库</b>

对于具备开源项目管理权限的开发者, 可以直接通过git push命令, 提交代码改动: 

    git push –u origin coron-4.4

在修改后的Git库使用上述命令。origin是远程仓库的别名, 是开发者自定义的, 也可以为其他别名； coron-4.4是改动的Git库所在的分支。

<b>2) 通过Code Review方式提交代码</b>

对于具备GitHub账户的开发者, 可以利用GitHub提供的Pull Request方式, 将代码改动以Code Review的形式, 发送给开源项目的管理者。待Code Review通过后, 代码改动将会合并到提交分支。

为了能够提交代码, 开发者需首先注册GitHub账户, 将baidurom的Git库Fork到自己的账户下； 然后, 对Git库进行代码修改, 发送Pull Request。最后, 在开源项目的管理者收到提交请求时, 会对代码进行Code Review, 如果符合准入标准, 就会将改动代码合并到主干分支中。

