---
title: 用Canvas编织璀璨星空图
categories: note
tags: [HTML, JS, Canvas]
date: 2017-07-16 10:42:10
updated: 2017-07-16 10:42:10
---
### 前言
在网上看到的这个项目，觉着挺炫，拿到源码后参照作者的教程弄明白了实现原理，原作者的[教程在这](http://www.jianshu.com/p/f5c0f9c4bc39)。在研究过程中感觉原教程缺了点儿东西，我觉得我可以写一篇更好的教程（傲娇脸.jpg）, 也算是对研究结果做个总结！

### 完成效果
[点击查看完成后的效果](https://zhangxiaoshang.github.io/projects/canvas%E7%BC%96%E7%BB%87%E7%92%80%E7%92%A8%E6%98%9F%E7%A9%BA/)

### 分析
1. 整个页面只用了一个canvas元素，canvas画布大小为页面可视区域大小，整个canvas的背景为黑色，在画布上只有两种元素：星星和连线;
2. 星星半径有大有小;
3. 距离近的星星之间的连线较粗且透明度较低，距离较远的相反，星星之间的距离太远则没有连线;
4. 星星是运动的，速度、方向不一；
5. 有一个星星的位置是跟随鼠标移动的；

### Coding
**HTML部分**

```html
<!DOCTYPE html>
<html>
<head>
    <title>用Canvas编织璀璨星空图</title>
</head>
<body>
    <canvas id="canvas"></canvas>

    <script type="text/javascript" src="index.js"></script>
</body>
</html>
```

以上就是全部html代码，只有一个canvas标签。

**JavaScript部分**

1.首先我们要得到那个 canvas 并得到绘制上下文：

```js
var canvasEl = document.getElementById('canvas');
var ctx = canvasEl.getContext('2d');
```

2.声明两个数组，分别存放星星和连线

```js
var nodes = [];     // 星星数组
var edges = [];     // 连线数组
```

3.构建星星

```js
// 构建星星
function constructorNodes() {
    for (var i = 0; i < 100; i++) {
        var node = {
            drivenByMouse: i === 1,
            x: Math.random() * canvasEl.width,
            y: Math.random() * canvasEl.height,
            vx: Math.random() - 0.5,
            vy: Math.random() - 0.5,
            radius: Math.random() > 0.9 ? Math.random() * 3 + 3 : Math.random() * 3 + 1
        };
        
        nodes.push(node);
    }
}
```
> for循环产生100个星星;

> drivenByMouse: 在一开始的项目分析中知道，整个系统中要有一个星星是跟随鼠标移动的，跟随鼠标移动的星星是不用绘制出来的，这里用drivenByMouse属性做标记，标记 `i === 0` 即第一个星星，后面遍历绘制星星的时候就可以根据这个属性的值判断当前星星是否需要绘制;

> x: 星星的x坐标;

> y: 星星的y坐标;

> vx: 星星在x轴方向的速度;

> vy: 星星在y轴方向的速度;

> radius: 星星的半径; 为了实现页面中小半径星星占多数，大半径星星占少数，用`Math.random() > 0.9`进行概率分配，使得10%的星星半径是`Math.random() * 3 + 3`即`3~6`, 90%的星星半径是`Math.random() * 3 + 1`即`1~4`;

> *在构建星星x,y坐标的时候用到了`canvasEl.clientWidth`和`canvasEl.clientHeight`, `canvas`画布的默认大小是宽300高150，需要设置一下画布大小，让画布充满屏幕，另外希望在改面浏览器窗口大小的时候，画布大小也随之改变，可以像下面这样做：*

4.设置画布大小

```js
window.onresize = function() {
    canvasEl.width = document.body.scrollWidth;
    canvasEl.height = document.body.scrollHeight;

    if (nodes.length === 0) {
        constructorNodes();
        constructorEdge();
    }

    render();
};

window.onresize()
```
>浏览器窗口大小改变会触发`window.onresize`;
>
>`document.body.scrollWidth`和`document.body.scrollHeight`分别是浏览器可视工作区的宽高;
>
>重置画布大小之后立即调用`render`函数绘制星空（render函数目前还没有定义，后面再具体实现函数内容）;
>
>在绘制星空之前需要检查`nodes`数组是否为空，如果为空则需要先调用构建函数`constructorNodes `和`constructorEdge `分别构建星星和连线;
>
>因为不曾在`canvas`标签设置画布宽高，所以需要在程序中手动调用一次`window.onresize`,实现对画布的初始化设置；

星星构建好了，画布大小也设置完成，下面开始构建连线

5.构建连线

```js
// 构建连线
function constructorEdge() {
    nodes.forEach(function(e1) {
        nodes.forEach(function(e2) {
            if (e1 === e2) {
                return;
            }

            var edge = {
                from: e1,
                to: e2
            };

            addEdge(edge);
        })
    })
}
```
>双重遍历`nodes`；
>
>如果两个节点是同一个，则跳过；
>
>设置两个星星之间连线的起点是`e1`终点是`e2`
>
>这里没有直接用`edges.push()`，为什么？
>
>假设之前连接了 A、B两点，也就是外侧循环是A，内侧循环是B，那么在下一次循环中，外侧为B，内侧为A，是不是也会创建一条线呢？而实际上，这两条线除了方向不一样以外是完全一样的，这完全没有必要而且占用资源。因此在 `addEdge` 函数中进行一个判断:

```js
function addEdge(edge) {
    var ignore = false;
    
    edges.forEach(function(e) {
        if (e.from === edge.to && e.to === edge.from) {
            ignore = true;
        }
    });
    
    if (!ignore) {
        edges.push(edge);
    }
}
```
整个系统所需要的元素：星星和联系都构建好了，下面就可以实现前面提到的绘制函数`render`了

6.实现绘制函数

这个函数内容有点多，分两步来介绍，首先是背景和星星的绘制：

```js
function render() {
    // 绘制背景
    ctx.fillStyle = backgroundColor;
    ctx.fillRect(0, 0, canvasEl.clientWidth, canvasEl.clientHeight);
    
    // 绘制星星
    nodes.forEach(function(node) {
        if (node.drivenByMouse) {
            return;
        }

        ctx.fillStyle = nodeColor;
        ctx.beginPath();
        ctx.arc(node.x, node.y, node.radius, 0, 2 * Math.PI);
        ctx.fill();
    })
}
```
>背景颜色存储在`backgroundColor`变量中，将所有全局变量的声明统一放在js文件的开始位置是个好主意，方便日后查找修改。顺便将后面要用到的`nodeColor`和`edgeColor`一起声明了。代码看起来像这样：

```js
var backgroundColor = '#000';
var nodeColor = '#fff';
var edgeColor = '#fff';
```

接着上面的代码，完成第二部分连线的绘制，整个函数完成后看起来像这样：

```js
function render() {
    // 绘制背景
    ctx.fillStyle = backgroundColor;
    ctx.fillRect(0, 0, canvasEl.clientWidth, canvasEl.clientHeight);

    // 绘制星星
    nodes.forEach(function(node) {
        if (node.drivenByMouse) {
            return;
        }

        ctx.fillStyle = nodeColor;
        ctx.beginPath();
        ctx.arc(node.x, node.y, node.radius, 0, 2 * Math.PI);
        ctx.fill();
    });

    // 绘制连线
    edges.forEach(function(edge) {
        var length  = lengthOfEdge(edge);
        var maxLength = canvasEl.clientWidth / 8;

        if (length > maxLength) {
            return;
        }

        ctx.strokeStyle = edgeColor;
        ctx.strokeWidth = (1 - length / maxLength) * 2.5;
        ctx.globalAlpha = 1 - length / maxLength;
        ctx.beginPath();
        ctx.moveTo(edge.from.x, edge.from.y);
        ctx.lineTo(edge.to.x, edge.to.y);
        ctx.stroke();
    });
    
    ctx.globalAlpha = 1.0;
}
```
>计算连线的长度，这里用了另一个函数`lengthOfEdge`，和之前一样，这个函数留着后面再具体实现；
>
>`maxLength`设置连线的最大长度是画布的1/8，超出这个长度的连线就不进行绘制了；
>
>`ctx.strokeStyle`设置画笔颜色,即连线的颜色，使用的`edgeColor`变量在前面声明过了；
>
>`ctx.strokeWidth`连线的宽度，可以不断调整公式后面的系数达到想要的效果；
>
>`ctx.globalAlpha`透明度设置，这是个全局变量，连线绘制完成后要重置为`1.0`，不然就出事儿了！

接着实现上面提到的用于计算连线长度的函数：

```js
function lengthOfEdge(edge) {
    return Math.sqrt(Math.pow(edge.from.x - edge.to.x, 2) + Math.pow(edge.from.y - edge.to.y, 2));
}
```
>看着挺复杂，其实就是求两点间的距离：d<sup>2</sup> = (x<sub>1</sub>-x<sub>2</sub>)<sup>2</sup> + (y<sub>1</sub> - y<sub>2</sub>)<sup>2</sup>

到这里星空绘制完成，打开页面就可以看到效果了，下面开始实现让星空动动动起来！

7.实现动画效果

这里使用`window.requestAnimationFrame(step)`实现动画效果，找到`window.onresize()`这句代码，在这句之后调用函数`window.requestAnimationFrame(step)`, 就像这样：

```js
window.requestAnimationFrame(step);
```

然后需要做的就是定义`step`函数：

```js
function step() {
    nodes.forEach(function(node) {
        node.x += node.vx;
        node.y += node.vy;
        
        // 越界检查
        if(node.x < 0 || node.x > canvasEl.clientWidth) {
            node.vx *= -1;
            node.x = clamp(0, canvasEl.clientWidth, node.x);
        }
        if (node.y < 0 || node.y > canvasEl.clientHeight) {
            node.vy *= -1;
            node.y = clamp(0, canvasEl.clientHeight, node.y)
        }
    });

    render();
    window.requestAnimationFrame(step);
}
```
>遍历所有星星，更新每个星星的x,y坐标，公式：x = x + vx; y = y + vy;
>
>星星坐标改变后要检查星星的坐标位置是否超出了画布边界，如果超出边界则设置速度为反方向，然后调用函数`clamp`,为星星坐标设置一个不超出边界的值；
>
>星星坐标改变后，立即调用绘制函数，重新绘制星空；
>
>绘制完成后再调用`window.requestAnimationFrame(step)`,这样就形成了一个循环，星空不断重绘，形成动画；

现在实现上面的`clamp`函数，就像是这样：

```js
function clamp(min, max, value) {
    if (value < min) {
        return min;
    }
    if (value > max) {
        return max;
    }
    return value;
}
```

OK, 进行到这里，整个系统基本完成了，现在再打开页面已经可以看到酷炫的效果了，不过还可以再酷炫一点儿，就是一开始提到的要有一个星星是跟随鼠标移动的，现在就来实现它：

8.实现鼠标跟随

定义一个全局变量 `mousePos`存储鼠标x,y位置，初始位置就设置为`[0,0]`, 和之前的全局变量放在一起，代码是这样的：

```js
var mousePos = [0, 0];
```

需要获取鼠标位置，这样实现：

```js
window.onmousemove = function(e) {
  mousePos[0] = e.clientX;
  mousePos[1] = e.clientY;
};
```

知道了鼠标位置就可以调整第一颗星星（即需要跟随鼠标移动的星星）的位置了：

```js
function adjustNodeDrivenByMouse() {
    nodes[0].x = mousePos[0];
    nodes[0].y = mousePos[1];
}
```
第一颗星星和其它星星的区别在于，其它星星是根据自身的`vx,vy`的值改变位置的，第一颗星星是根据鼠标位置改变位置的；相同的是它们都需要不断更新自己的位置，所以要在`step`函数中调用`adjustNodeDrivenByMouse `函数来更新第一个星星的位置。找到`step`函数，在`step`函数里有一个绘制函数`render`, 把`adjustNodeDrivenByMouse `函数的调用写在绘制函数`render`调用前面，就是这句：

```js
adjustNodeDrivenByMouse();
```

现在基本就完成了，已经可以看到不断变化的星空，鼠标跟随效果，但是上面的`adjustNodeDrivenByMouse `函数还可以优化一下，需要优化的原因是这样的：

>刷新页面，此时第一颗星星的位置一开始设置的是`[0,0]`,这时如果鼠标从浏览器外移动到星空的一个位置，会发现跟随星空移动的点（即第一颗星星）瞬间就到了鼠标位置，优化的目的就是让这个移动有个过渡的过程，而不是瞬移。现在来修改`adjustNodeDrivenByMouse `函数，修改后是这样：

```js
function adjustNodeDrivenByMouse() {
    // nodes[0].x = mousePos[0];
    // nodes[0].y = mousePos[1];
    nodes[0].x += (mousePos[0] - nodes[0].x) / easingFactor;
    nodes[0].y += (mousePos[1] - nodes[0].y) / easingFactor;
}
```
>这里将之前的代码注释了，便于对照。新增的两行用到了`easingFactor`变量，这是个过渡系数，这里设置的是`5`，和之前一样，也将这个变量和其他全局变量放在一起：

```js
var easingFactor = 5;
```
>修改`adjustNodeDrivenByMouse `函数后，刷新页面，将鼠标移出浏览器，然后再移入到浏览器时就会发现跟随鼠标移动的点从原始位置移动到鼠标位置时是有一个平滑的过渡过程的。

最后奉上完整的`index.js`代码：

```js
var canvasEl = document.getElementById('canvas');
var ctx = canvasEl.getContext('2d');

var backgroundColor = '#000';
var nodeColor = '#fff';
var edgeColor = '#fff';

var nodes = [];     // 星星数组
var edges = [];     // 连线数组
var mousePos = [0, 0];
var easingFactor = 5;

// 构建星星
function constructorNodes() {
    for (var i = 0; i < 100; i++) {
        var node = {
            drivenByMouse: i === 1,
            x: Math.random() * canvasEl.width,
            y: Math.random() * canvasEl.height,
            vx: Math.random() - 0.5,
            vy: Math.random() - 0.5,
            radius: Math.random() > 0.9 ? Math.random() * 3 + 3 : Math.random() * 3 + 1
        };

        nodes.push(node);
    }
}

// 构建连线
function constructorEdge() {
    nodes.forEach(function(e1) {
        nodes.forEach(function(e2) {
            if (e1 === e2) {
                return;
            }

            var edge = {
                from: e1,
                to: e2
            };

            addEdge(edge);
        })
    })
}

function addEdge(edge) {
    var ignore = false;

    edges.forEach(function(e) {
        if (e.from === edge.to && e.to === edge.from) {
            ignore = true;
        }
    });

    if (!ignore) {
        edges.push(edge);
    }
}

function render() {
    // 绘制背景
    ctx.fillStyle = backgroundColor;
    ctx.fillRect(0, 0, canvasEl.clientWidth, canvasEl.clientHeight);

    // 绘制星星
    nodes.forEach(function(node) {
        if (node.drivenByMouse) {
            return;
        }

        ctx.fillStyle = nodeColor;
        ctx.beginPath();
        ctx.arc(node.x, node.y, node.radius, 0, 2 * Math.PI);
        ctx.fill();
    });

    // 绘制连线
    edges.forEach(function(edge) {
        var length  = lengthOfEdge(edge);
        var maxLength = canvasEl.clientWidth / 8;

        if (length > maxLength) {
            return;
        }

        ctx.strokeStyle = edgeColor;
        ctx.strokeWidth = (1 - length / maxLength) * 2.5;
        ctx.globalAlpha = 1 - length / maxLength;
        ctx.beginPath();
        ctx.moveTo(edge.from.x, edge.from.y);
        ctx.lineTo(edge.to.x, edge.to.y);
        ctx.stroke();
    });

    ctx.globalAlpha = 1.0;
}

function lengthOfEdge(edge) {
    return Math.sqrt(Math.pow(edge.from.x - edge.to.x, 2) + Math.pow(edge.from.y - edge.to.y, 2));
}

function step() {
    nodes.forEach(function(node) {
        node.x += node.vx;
        node.y += node.vy;

        // 越界检查
        if(node.x < 0 || node.x > canvasEl.clientWidth) {
            node.vx *= -1;
            node.x = clamp(0, canvasEl.clientWidth, node.x);
        }
        if (node.y < 0 || node.y > canvasEl.clientHeight) {
            node.vy *= -1;
            node.y = clamp(0, canvasEl.clientHeight, node.y)
        }
    });

    adjustNodeDrivenByMouse();
    render();
    window.requestAnimationFrame(step);
}

function clamp(min, max, value) {
    if (value < min) {
        return min;
    }
    if (value > max) {
        return max;
    }
    return value;
}

function adjustNodeDrivenByMouse() {
    // nodes[0].x = mousePos[0];
    // nodes[0].y = mousePos[1];
    nodes[0].x += (mousePos[0] - nodes[0].x) / easingFactor;
    nodes[0].y += (mousePos[1] - nodes[0].y) / easingFactor;
}

window.onmousemove = function(e) {
  mousePos[0] = e.clientX;
  mousePos[1] = e.clientY;
};

window.onresize = function() {
    canvasEl.width = document.body.scrollWidth;
    canvasEl.height = document.body.scrollHeight;

    if (nodes.length === 0) {
        constructorNodes();
        constructorEdge();
    }

    render();
};

window.onresize();
window.requestAnimationFrame(step);


```

好了，整个程序的介绍到此结束，Have Fun!