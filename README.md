# SoDiaoEditor.v2
> 更多精彩请移步[博客园文章](http://www.cnblogs.com/tlzzu/p/6654208.html)，阅读英文版请移步[这里](https://github.com/tlzzu/SoDiaoEditor.v2/blob/master/README-en.md)
## [预览](http://editor.sodiao.org/example/index.html)
1. [设计模式--电子病历设计器(扩展toolbar)](http://editor.sodiao.org/example/design-design.html)
```
建议给病历模板设计者（开发人员，或者科主任）使用。
可用来设计电子病历模板，也可以当做电子病历编辑器使用。
此时输入的值可利用SDE对象暴露出的接口获取。
```
2. [编辑模式--电子病历设计器](http://editor.sodiao.org/example/editor-design.html))
```
建议给医生使用。
此时医生可以在原有模板中添加已有的控件，也可以给科主任用来设计模板。
亦可通过SDE对象暴露出来的接口获取数据。
```
3. [只读模式--电子病历设计器](http://editor.sodiao.org/example/readonly-design.html))
```
建议该模式给医生查看使用，在该模式下电子病历中所有控件均不可编辑。
```
4. [按钮控制--电子病历设计器](http://editor.sodiao.org/example/btn-design.html)
```
按钮通过事件控制！
```
5. [编辑模式--电子病历编辑器](http://editor.sodiao.org/example/editor-editor.html)
```
建议给医生使用，此模式下医生只能编辑控件中的值，其余均不可修改。
```
6. [只读模式--电子病历编辑器](http://editor.sodiao.org/example/readonly-editor.html)
```
该模式只允许查看，控件不可被编辑。
```
7. [按钮控制--电子病历编辑器](http://editor.sodiao.org/example/btn-editor.html)
```
按钮通过事件控制！
```
## 更新
本次更新以下内容：
1. 新增以下toolbar：
```
编辑
  i. 剪切板
      1. 复制、粘贴、切剪
  ii. 字体
      1. 字体、字号、增大字体、缩小字体
插入
  i. 字符
  ii. 链接
      1. 取消链接
  iii. 图片
      1. 图片、涂鸦
  iv. 表格
表格
  i. 表格
      1. 插入表格
```
2. 设计器中新增SED对象，并增加以下接口：
3. 修复事件兼容性（暂不支持IE8及其以下的浏览器，后续会有解决方案）
4. 增加toolbar可配置性。
5. 解决上一个版本中的bug。
6. 等
## 结构
```shell
data                    模拟异步请求的数据，正式项目中可忽略
dialogs                 扩展百度ueditor的dialogs
dist                    核心js文件
    js
        sde.design.js   SoDiaoEditor设计器核心js
        sde.editor.js   SoDiaoEditor编辑器核心js
example                 一些demo
ueditor                 百度ueditor库，可替换成自己版本
sde.config.js           核心配置文件
```
## 使用
##### 电子病历设计器：
```
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>设计模式--电子病历设计器</title>
    <!-- 注意以下各脚本之间的加载顺序！ -->
    <script type="text/javascript" src="sde.config.js"></script>
    <link rel="stylesheet" href="ueditor/themes/default/css/ueditor.css" />
    <script type="text/javascript" src="ueditor/ueditor.all.js"></script>
    <script type="text/javascript" src="ueditor/lang/zh-cn/zh-cn.js"></script>
    <script type="text/javascript"  src="dist/js/sde.design.js"></script>
</head>
<body>
    <script id="myEditor" type="text/plain" style="width:680px;height:1000px;">
        病历模板...
    </script>
    <script type="text/javascript">
        window.onload = function() {
            var sde = new SDE({
                id: "myEditor",
                title: '<div style="height: 45px;overflow: hidden;background-color: #16742B;">' +
                    '<div class="left" style="position:absolute;top:0;left:0;">' +
                    '<img src="../data/img/SoDiaoL.png" style="height:35px;margin:5px;border:none;" />' +
                    '</div>' +
                    '<h1 style="font-size: 14px;height: 45px;line-height: 45px;margin: 0 auto;text-align: center;font-weight: normal;color:#fff;" >SoDiaoEditor电子病历编辑器</h1>' +
                    '</div>', //自定义title
            });
        };
    </script>
</body>
</html>
```
###### 注意：
> 各个脚本之间的加载顺序，已本例为准。
配置项(sde.config.js)：
```
/*
默认配置项
*/
(function() {
    var URL = window.UEDITOR_HOME_URL || getUEBasePath();
    /*
    SDE_CONFIG 配置项
    */
    window.SDE_CONFIG = {
        HOME_URL: URL,
        HOME_URL_DIALOGS: URL + 'dialogs/',//SoDiaoEditor扩展ueditor的dialogs位置
        EDITOR_URL: URL + 'dist/js/sde.editor.js',
        MODE: "DESIGN", //DESIGN:设计|EDITOR:编辑|READONLY:只读（所有节点都不可编辑）
        CONTROL_TEMPLATES: []//控件模板
    };
    /**
     * 配置项主体。注意，此处所有涉及到路径的配置别遗漏URL变量。
     */
    window.UEDITOR_CONFIG = {
        UEDITOR_HOME_URL: URL + 'ueditor/', //为编辑器实例添加一个路径，这个不能被注释
        serverUrl: URL + "data/config.json", //URL + "net/controller.ashx", // 服务器统一请求接口路径
        toolbars: [], //工具栏上的所有的功能按钮和下拉框，可以在new编辑器的实例时选择自己需要的重新定义
        autoClearinitialContent: false, //是否自动清除编辑器初始内容，注意：如果focus属性设置为true,这个也为真，那么编辑器一上来就会触发导致初始化的内容看不到了
        //iframeJsUrl: URL + window.SDE_CONFIG.EDITOR_URL + '?temp=' + new Date().getTime(), //给编辑区域的iframe引入一个js文件
        // iframeCssUrl: URL + 'EMR/css/default.css?temp=' + new Date().getTime(), //给编辑区域的iframe引入一个css文件
        allowDivTransToP: false, //允许进入编辑器的div标签自动变成p标签
        wordCount: false, //关闭字数统计
        elementPathEnabled: false, //关闭elementPath
        autoClearinitialContent: false
    };
    function getUEBasePath(docUrl, confUrl) {
        return getBasePath(docUrl || self.document.URL || self.location.href, confUrl || getConfigFilePath());
    }
    function getConfigFilePath() {
        var configPath = document.getElementsByTagName('script');
        return configPath[configPath.length - 1].src;
    }
    function getBasePath(docUrl, confUrl) {
        var basePath = confUrl;
        if (/^(\/|\\\\)/.test(confUrl)) {
            basePath = /^.+?\w(\/|\\\\)/.exec(docUrl)[0] + confUrl.replace(/^(\/|\\\\)/, '');
        } else if (!/^[a-z]+:/i.test(confUrl)) {
            docUrl = docUrl.split("#")[0].split("?")[0].replace(/[^\\\/]+$/, '');
            basePath = docUrl + "" + confUrl;
        }
        return optimizationPath(basePath);
    }
    function optimizationPath(path) {
        var protocol = /^[a-z]+:\/\//.exec(path)[0],
            tmp = null,
            res = [];
        path = path.replace(protocol, "").split("?")[0].split("#")[0];
        path = path.replace(/\\/g, '/').split(/\//);
        path[path.length - 1] = "";
        while (path.length) {
            if ((tmp = path.shift()) === "..") {
                res.pop();
            } else if (tmp !== ".") {
                res.push(tmp);
            }

        }
        return protocol + res.join("/");
    }
    window.UE = {
        getUEBasePath: getUEBasePath
    };
})();
```
注意：
> 请重点关注window.SDE_CONFIG 和 window.UEDITOR_CONFIG 。 建议window.UEDITOR_CONFIG不要修改，可根据需求该window.SDE_CONFIG对象
##### 电子病历编辑器：

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>编辑模式--电子病历编辑器</title>
    <script type="text/javascript" src="dist/js/sde.editor.js"></script>
</head>
<body>
    <div id="myEditor" style="width:680px;height:1000px;margin:0 auto;">
        病历内容...
    </div>
    <script type="text/javascript">
        window.onload = function() {
            var sde = new SDE({
                id: "myEditor",
                title: '<div style="height: 45px;overflow: hidden;background-color: #16742B;">' +
                    '<div class="left" style="position:absolute;top:0;left:0;">' +
                    '<img src="../data/img/SoDiaoL.png" style="height:35px;margin:5px;border:none;" />' +
                    '</div>' +
                    '<h1 style="font-size: 14px;height: 45px;line-height: 45px;margin: 0 auto;text-align: center;font-weight: normal;color:#fff;" >SoDiaoEditor电子病历编辑器</h1>' +
                    '</div>', //自定义title
                mode: 'EDITOR'//配置模式
            });
        };
    </script>
</body>
</html>
```





## 文档
##### 电子病历设计器：
方法 | 说明 | 描述
---|---|---
ready(function(){}) | 编辑器加载完成 | (之后的所有方法必须在ready加载完成后使用）
html([html]) | 获取/设置所有编辑器中的html模板 | 如果html不传递，则为获取，有值则为设置
getControl([id]) | 获取编辑器中的控件 | id为可选，若为无则是获取所有控件
setControl(ctl) | 设置编辑器中指定id的控件值 | ctl：{ID:'',VALUE:''}如果是select控件类型ctl：{ID:'',VALUE:'',TEXT:''}。ctl可以为数组也可以为对象，设置冻结REQUIRED：1为冻结，只读不可操作
setMode(mode) | 设置编辑器模式 | mode可选：DESIGN（设计）、EDITOR（编辑）、READONLY（只读）
##### 电子病历编辑器：
方法 | 说明 | 描述
---|---|---
getControl([id]) | 获取编辑器中的控件 | id为可选，若为无则是获取所有控件
setControl(ctl) | 设置编辑器中指定id的控件值 | ctl：{ID:'',VALUE:''}如果是select控件类型ctl：{ID:'',VALUE:'',TEXT:''}。ctl可以为数组也可以为对象，设置冻结REQUIRED：1为冻结，只读不可操作
setMode(mode) | 设置编辑器模式 | mode可选：DESIGN（设计）、EDITOR（编辑）、READONLY（只读）
## 贡献&bug提交
1. 可邮件至[dd@sodiao.org](mailto://dd@sodiao.org/)；
2. 可以在github中的Iss中提交；
## 打赏
![image](data/img/ds.png)