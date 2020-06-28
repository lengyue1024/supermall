<img align="right" src="public/favicon.ico" width="150">

# SuperMall
> vuejs project 

## TreeStructure
> 此处添加目录树


[注]：<https://github.com/liuchengxu/git-commit-emoji-cn>
<https://www.cnblogs.com/Chendaqian/p/11958201.html>
<https://www.jianshu.com/p/dd480882b483>
<https://www.cnblogs.com/webrqy/p/9939231.html>

解决问题：
[解决Github网页上图片显示失败的问题](https://blog.csdn.net/qq_38232598/article/details/91346392)

本次修改了 `C:\Windows\System32\drivers\etc\hosts`文件



## 目录结构

划分目录结构

views: 视图
components：公共组件
    common：剥离出最公共的组件（其他项目也可以直接拿来复用，不依赖于本项目）
    content：和本项目相关的公共组件（依赖于本项目）
assets：css文件和图片资源
router：路由
store：状态管理
network：网络相关
common：可复用的js文件


使用normalize.css
