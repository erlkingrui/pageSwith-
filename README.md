# pageSwith-如何使用
图片轮播
使用时引入pageSwitch.js


// 首先在页面中引入pageSwitch.js
// 调用 pageSwitch 方法

var pw=new pageSwitch('container id',{
    duration:600,           //int 页面过渡时间
    direction:1,            //int 页面切换方向，0横向，1纵向
    start:0,                //int 默认显示页面
    loop:false,             //bool 是否循环切换
    ease:'ease',            //string|function 过渡曲线动画，详见下方说明
    transition:'slide',     //string|function转场方式，详见下方说明
    freeze:false,           //bool 是否冻结页面（冻结后不可响应用户操作，可以通过 `.freeze(false)` 方法来解冻）
    mouse:true,             //bool 是否启用鼠标拖拽
    mousewheel:false,       //bool 是否启用鼠标滚轮切换
    arrowkey:false,         //bool 是否启用键盘方向切换
    autoplay:false,         //bool 是否自动播放幻灯 新增
    interval:int            //bool 幻灯播放时间间隔 新增
});

//调用方法
pw.prev();                  //上一张
pw.next();                  //下一张
pw.slide(n);                //第n张
pw.setEase();               //重新设定过渡曲线
pw.setTransition();         //重新设定转场方式
pw.freeze(true|false);      //冻结页面转换，冻结后不可响应用户操作（调用slide prev next方法还可以进行）

pw.play();                  //播放幻灯
pw.pause();                 //暂停幻灯

/* 2015.03.22 新增方法 */
pw.prepend(DOM_NODE);       //前增页面
pw.append(DOM_NODE);        //后增页面
pw.insertBefore(DOM_NODE,index);    //在index前添加
pw.insertAfter(DOM_NODE,index);     //在index后添加
pw.remove(index);           //删除第index页面
pw.isStatic();              //是否静止状态

pw.destroy();               //销毁pageSwitch效果对象

/* 事件绑定
 * event可选值:
 * 
 * before 页面切换前
 * after 页面切换后
 * update 页面切换中
 * dragStart 开始拖拽
 * dragMove 拖拽中
 * dragEnd 结束拖拽
 */
pw.on(event,callback);

setEase 示例

内置支持 linear ease ease-in ease-out ease-in-out bounce等
bounce 弹跳过渡，很有意思，可以试试

//注：此处传值也可直接在new pageSwitch对象时经ease参数传入
//设置匀速linear过渡示例：
pw.setEase('linear'); //由于内置了linear支持，所以可以直接使用

//假如没有内置linear，则使用自定义过渡曲线函数如下
pw.setEase(function(t,b,c,d){
    return c*t/d + b;
});

更多曲线函数参见：https://github.com/zhangxinxu/Tween/blob/master/tween.js
setTransition 示例

支持以下转场效果：

fade 渐隐渐显 | demo

slice 裁切效果 | demo

scroll 页面滚动 | demo
scroll3d 3d页面滚动 | demo
scrollCover 页面视差滚入滚出（前后页面速度不一致） | demo
scrollCoverReverse同上反向 | demo
scrollCoverIn 总是下一张页面视差滚入 | demo
scrollCoverOut 总是当前页面视差滚出 | demo

slide 滑动切换，后者页面有缩放效果 | demo
slideCover 页面滑入滑出 | demo
slideCoverReverse 同上反向 | demo
slideCoverIn 总是下一张页面滑入 | demo
slideCoverOut 总是当前页面滑出 | demo

flow 封面滑动切换 | demo
flowCover 页面滑入滑出 | demo
flowCoverReverse 同上反向 | demo
flowCoverIn 总是下一张页面滑入 | demo
flowCoverOut 总是当前页面滑出 | demo

zoom 缩放切换 | demo
zoomCover 页面缩进缩出 | demo
zoomCoverReverse 同上反向 | demo
zoomCoverIn 总是下一张页面缩入 | demo
zoomCoverOut 总是当前页面缩出 | demo

skew 扭曲切换 | demo
skewCover 页面扭进扭出 | demo
skewCoverReverse 同上反向 | demo
skewCoverIn 总是下一张页面扭入 | demo
skewCoverOut 总是当前页面扭出 | demo

flip 翻转切换 | demo
flip3d 3d翻转切换 | demo
flipClock 翻页钟效果 | demo
flipPaper 翻书本效果 | demo
flipCover 页面翻入翻出 | demo
flipCoverReverse 同上反向 | demo
flipCoverIn 总是下一张页面翻入 | demo
flipCoverOut 总是当前页面翻出 | demo

bomb 放大切换 | demo
bombCover 页面大入大出 | demo
bombCoverReverse 同上反向 | demo
bombCoverIn 总是下一张页面大入 | demo
bombCoverOut 总是当前页面大出 | demo

注意：除了fade，所有效果都支持指定X或Y轴方向效果，只要在名字后面加上X或Y即可。 例如：scrollY flipX flipCoverX flipCoverInX 等类似。

//注：此处传值也可直接在new pageSwitch对象时经transition参数传入
//设置fade效果示例：
pw.setTransition('fade'); //由于内置了fade效果，所以可以直接调用。

//假定没有内置fade，自定义转场函数如下
pw.setTransition(function(cpage,cp,tpage,tp){
    /* 过渡效果处理函数
     * @param HTMLElement cpage 参与动画的前序页面
     * @param Float cp 目标页面过渡比率，取值范围-1到1
     * @param HTMLElement tpage 参与动画的后序页面；如果非循环loop模式，则在切换到边缘页面时可能不存在该参数
     * @param Float tp 目标页面过渡比率，取值范围-1到1；如果非循环loop模式，则在切换到边缘页面时可能不存在该参数
     */

    if('opacity' in cpage.style){
        cpage.style.opacity=1-Math.abs(cp);
        if(tpage){
            tpage.style.opacity=Math.abs(cp);
        }
    }else{
        cpage.style.filter='alpha(opacity='+(1-Math.abs(cp))*100+')';
        if(tpage){
            tpage.style.filter='alpha(opacity='+Math.abs(cp)*100+')';
        }
    }
});

//如果你有jQuery类似组件，可以更简单
pw.setTransition(function(cpage,cp,tpage,tp){
    $(cpage).css('opacity',1-Math.abs(cp));
    $(tpage).css('opacity',Math.abs(cp));
});

jQuery/Zepto适配器

$.fn.extend({
    pageSwitch:function(cfg){
        this[0].ps=new pageSwitch(this[0],cfg);
        return this;
    },
    ps:function(){
        return this[0].ps;
    }
});

//使用
$(container_id).pageSwitch({
    duration:1000,
    transition:'slide'
});

$(container_id).ps().next(); //由于链式调用返回依然是jq对象自身，所以如果需要使用pageSwitch对象方法，需要通过 `.ps()`
