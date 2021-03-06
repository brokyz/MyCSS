# CSS定位

[toc]

## 为什么需要定位

有些元素的位置比较刁钻，使用标准流和浮动很难实现。定位为我们解决了这个困难。

<span style="color:red">定位可以让某一元素自由的在一个盒子中移动位置或者固定屏幕中某个位置，并且压住其他盒子。</span>



## 定位组成

定位：将盒子定在某一个位置，所以定位也是在摆放盒子，按照定位的方式移动盒子。

==定位 = 定位模式 + 边偏移==

定位模式用于指定一个元素在文档中的定位方式。边偏移则决定了该元素的最终位置。

## 边偏移

边偏移就是定位盒子移动到最终位置。有`top、bottom、left、right`四个属性。

| 边偏移属性 | 示例         | 描述                                                 |
| ---------- | ------------ | ---------------------------------------------------- |
| top        | top: 80px    | **顶端**偏移量，定位元素相对于父元素**上边线的距离** |
| bottom     | bottom: 80px | **底部**偏移量，定位元素相对于父元素**下边线的距离** |
| left       | left: 80px   | **左侧**偏移量，定位元素相对于父元素**左边线的距离** |
| right      | right: 80px  | 右侧偏移量，定位元素相对于父元素右边线的距离         |

注意：left和right同时存在时并不冲突，不会层叠。当两个属性同时存在时优先选择left。同理top和bottom优先选择top。

## 定位模式

定位模式决定元素的定位方式，它通过CSS的 `position` 属性来设置，一共有四个可选值：

| 值       | 语义     |
| -------- | -------- |
| static   | 静态定位 |
| relative | 相对定位 |
| absolute | 绝对定位 |
| fixed    | 固定定位 |

### 静态定位

静态定位是元素的默认定位方式，就是无定位的意思。

语法：

```
选择器 { position: static; }
```

- 静态定位按照标准流特性摆放位置，它没有边偏移
- 静态定位在布局中很少使用

### 相对定位（重要）

==相对定位==是元素在移动位置的时候，是相对于它==原来的位置==来说的（自恋型）

语法：

```
选择器 { position: relative; }
```

相对定位的特点：（务必记住）

1. 他是相对于自己原来的位置来移动的（==移动位置的时候参照点是自己原来的位置==）。
2. 原来在标准流的位置继续占有，后面的盒子仍然以标准流的方式对待它。（==不托标，继续保留原来位置==）

因此，相对定位并没有脱标。它最典型的应用是给绝对定位当爹的。。

### 绝对定位 absolute （重要）

绝对定位是元素在移动位置的时候，是相对于它祖先元素来说的（拼爹型）。

语法：

```
选择器 { position: absolute; }
```

绝对定位的特点：（==务必记住==）

1. 如果==没有祖先元素==或者==祖先元素没有定位==，则以浏览器为准定位（Document文档）。
2. 如果祖先元素有定位（相对、绝对、固定定位），则以最近一级的有定位的祖先元素为参考点移动位置。
3. 绝对定位==不再占有原来的位置==。（脱标）

#### 子绝父相

子绝父相非常重要，意思是：==子级使用绝对定位的话，父级要用相对定位==

1. 子级绝对定位，不会占有位置，可以放到父盒子里面的任何一个地方，不会影响其他盒子。
2. 父盒子需要加定位限制子盒子在父盒子内显示。
3. 父盒子布局时，需要占有位置，因此父亲只能是相对定位。

这就是子绝父相的由来，所以==相对定位经常用来作为绝对定位的父级==。

总结：==因为父级需要占有位置，因此是相对定位，子盒子不需要占有位置，则是绝对定位==。

#### 绝对定位盒子居中

加了绝对定位的盒子不能通过 margin: 0 auto; 水平居中，但是可以通过下面的计算方法实现水平和垂直居中。

方法：

1. 先给盒子增加绝对定位
2. 让盒子的绝对定位走页面宽度的一半
3. 让盒子往左边走自己的一半

```
. box {
	position: absolute;
	left: 50%;
	margin-left: -100px
	width: 200px;
	height: 200px;
	background-color: pink;
}
```





### 固定定位

==固定定位==是元素==固定于浏览器可视区的位置==。

主要使用场景：可以在浏览器页面滚动时元素的位置不会改变。

语法：

```
选择器 {position: fixed; }
```

固定定位的特点：（务必记住）

1. 以浏览器的可视窗口为参照点移动元素。
   - 跟父元素没有任何关系
   - 不随滚动条的滚动而滚动

2. 固定定位==不占有原来的位置==
   - 固定定位也是脱标的，其实固定定位也可以看作是一种特殊的绝对定位。

#### 固定定位小技巧 贴近版心右侧

小算法：

 	1. 让固定定位的盒子 left：50%，走到浏览器可视区（也可以看作版心）的一半位置。
 	2. 让固定定位的盒子 margin-left: 版心宽度的一半巨鹿。多走 版心宽度的一般位置。

就可以让固定定位的盒子贴着版心对齐了。

### 粘性定位

==粘性定位==可以被认为是相对定位和固定定位的混合。Sticky粘性的：

语法：

```
选择器 {position: sticky; top: 10px; }
```

粘性定位的特点：

1. 以浏览器的可视窗口为参照点移动元素（固定定位特点）
2. 粘性定位==占有原来的位置==（相对定位特点）
3. 必须添加 top、left、bottom、right 其中一个才有效

==不常用，兼容性差，ie不支持，常用的类似效用使用JS来完成==



### 定位的叠放次序 z-index

在使用定位布局时，可能会出现盒子重叠的情况。此时可以使用 `z-index`来控制盒子的前后顺序（z轴）

语法：

```
选择器 { z-index: 1; }
```

- 数值可以是正整数、负整数或0，默认是auto，数值越大，盒子越靠上。
- 如果属性相同，则按照书写顺序，后来者居上。
- 数字后面不能加单位。
- 只有定位的盒子才有z-index属性



## 定位的页数属性

绝对定位和固定定位也和浮动类似。

1. 行内元素添加绝对定位或者固定定位，可以直接设置高度和宽度。
2. 块级元素添加绝对或者固定定位，如果不给宽度或者高度，默认大小是内容大小。
3. 脱标的盒子不会发生外边距塌陷
   - 浮动元素、绝对定位（固定定位）元素都不会出发外边距合并的问题。

## 定位的拓展

### 绝对定位（固定定位）会完全压住盒子

浮动元素不同，浮动元素只会压住它下面标准流的盒子，但是不会压住下面标准流盒子里面的文字（图片）

但是绝对定位（固定定位）会压住下面标准流所有的内容。

浮动之所以不会压住文字，因为浮动产生的最终目的是为了做文字环绕效果的。文字会围绕浮动元素。
