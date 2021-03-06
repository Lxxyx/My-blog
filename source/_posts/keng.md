---
title: 移动端项目“五一E起游”开发中遇到的坑总结
date: 2016-10-02
tags: ["css","移动web","javascript"]
---
## 前言
这次的博客内容来源于我的第一个完整的移动端项目练习[游客意见数据统计](https://github.com/maruko0713/Travel_comment_website)的开发过程总结，集中纪录了几个把我折腾的够呛的坑。在github上的readme里面虽然已经有一份了，但是还是决定把它写成博客，希望能帮更多的人减少踩坑的经历^_^_

<!-- more --> 
## 开发笔记
### 坑
先看这段代码:
css部分:    

```css
.mask {
    transition:1s;
}
```

js部分

```js
addClass(oMask,"pageShow");
oMask.style.opacity=1;  
```

我设置了mask模块的转换完成时间为1s，js部分中pageShow的作用是把原来的display:none转换成display:block,目的是先把这个模块显示出来，然后再让它完成一个1s之内从opacity为0到opacity为1的转变。    
然而结果并没有如我所愿，而是很简单粗暴地直接显示出来了，没有淡入的那个过程，查了一下资料，自己现在理解的原因是： 
>当display在none和block之间转换的时候，需要时间去渲染，这段代码里，在设置了display:block之后立刻设置了opacity:1,这时浏览器还没把模块渲染出来，就读取了opacity＝1的设定，然后在渲染出来那一刻它就是不透明的了，这个转换就体现不出来了。    

因此我们需要给它留出渲染的时间，等渲染结束了，再去设置opacity，这时候transition就是起作用的了:    

```js
addClass(oMask,"pageShow");
setTimeout(function(){
    oMask.style.opacity=1;  
},14);
```

至于这个时间为啥选14，因为在网上查到各浏览器的最小延迟时间都在10-15微秒之间，我就取了个14，这里主要是想取一个尽可能小的时间延迟，就算是这么小的时间延迟，也已经够浏览器渲染出这个模块了。（亲测）    
    
### 坑    
chrome的模拟器虽然很方便，但是还是不能完全的信任啊啊啊啊，今天用我的5s（难道是因为我的手机太旧？？？）测试了一下，发现input里面的button类型标签在重置了默认样式之后依然无法还原pc端测试出来的效果，一开始我是这样重置的:    
```css
input,textarea{
    outline:none;
    border:none;
    margin:0;
    padding:0;
}
```
![按钮图片](http://7xl4oh.com1.z0.glb.clouddn.com/79FC9D6F-7566-48BC-85DB-67DAC8B92EFF.png)    
然后结果呢，反正不长这样，特别丑，为了说明差距很大我用手机截了个屏幕:    
![浏览器默认样式按钮](http://7xl4oh.com1.z0.glb.clouddn.com/22C3E7C5-59A9-42B4-858B-04F790CE5BA8.png)    
一脸懵比，这差距大到我想不出来该去重置哪个css属性！    
论科学上网的重要性，后来我搜出了一个办法，只需一行：
```css
 -webkit-appearance:none; 
 ```
 就可以干掉手机safari下独有的这个bug哈哈哈哈。    
 今晚等室友回来再用她的6测试一下，看一下是不是我手机系统太老的问题.   
 填坑：不是系统的问题，6里的safari也这个德行，必须-webkit-appearance:none;才行   
 vivo浏览器测试正常(是的我把室友的手机全试了一遍啊哈哈哈)    

### 大坑    
大坑就是搞了很久才搞明白而且非常诡异的坑，今天上午我就遇到一个.
#### label标签里被隐藏的input会骗人
这个事情不单独搞出来讲绝对不能解恨，项目里面我在标签选择那一块写了一个事件委托，如果用户戳到span或者label标签那么就去判断input(input我设置了display:none)的checked属性,如果checked属性为真，那么label标签就去掉active状态，反之加上active状态，从而实现一个状态切换的效果。    
思路看起来好像没问题，但是在我操作的过程中会出现我明明点上了label，但是不变色的现象(active状态是要变色的)，为此，我测试了那一块代码我能测试的几乎所有地方，但是就是改不掉。后来干脆单独写了个demo来检查label标签，它的狐狸尾巴就露出来了: 

html部分:

~~~html 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>test</title>
    <meta name="viewport" content="width=200">
</head>
<body>  
    <label>
        <input type="checkbox">
        <span>测试</span>
    </label>
</body>
</html>
~~~  
    
css部分:    
```css
label {
    border:1px solid black;
    padding:10px;
    margin:10px;
}
input {
    display:none;
}
```

js部分:   
```js
window.onload = function() {
    var oLabel = document.getElementsByTagName("label")[0];
    oLabel.addEventListener("touchend",function(event){
        var oInput = document.getElementsByTagName("input")[0];
        console.log(oInput.checked);
        console.log(event.target);
    });
    document.addEventListener("touchend",function(event) {
        console.log(event.target.tagName);
    })
}
```

布局长这样:    
![label标签](http://7xl4oh.com1.z0.glb.clouddn.com/input.png)  
注意我戳的位置（是戳不是点，事件名称是touchend,click的话不存在这个bug，因为click你只能点到一个点，而touch却能覆盖一小片区域了）    
![加了范围演示的label标签](http://7xl4oh.com1.z0.glb.clouddn.com/input2.png)    
注意小圆点，这个小圆点（就是你戳手机的时候手指覆盖的范围）一旦有大于一半的面积滞留在label标签外面，神奇的事情就会发生，这时label标签的touch事件不会被触发，但是input的checked值会被改变，别问我为什么，浏览器非这么干，整整一个上午的测试告诉我，就是这样！    
#### 解决办法：    
现在我的办法是，input的checked值既然和touch事件是不同步的，那么我就不用它进行判断。在touch事件被触发的时候，我去判断一个自定义属性，这里我给input新增了一个on属性，on属性只有在touch事件触发的时候才改变，把这个地方改了之后，状态切换就变得很灵敏了。    


 
