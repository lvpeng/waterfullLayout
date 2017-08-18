## 该项目初衷
 > 昨天去一家公司面试过程中，聊到关于瀑布流的改进方式相关的的问题， 之前在前端过程中没有遇到此场景的，于是回家后也在思考瀑布流布局实现方式， 也算是对面试的一个学习总结

## 实现过程
 准备用纯CSS实现类似于「百度图片」那样子的横向瀑布流： 高度固定， 横向铺满
1. 初始想用到的是`float: left`方式实现，可是发现图片尺寸不一致的时候 会有「卡住」的空白间隔

2. 然后设置图片等高度， 会解决掉 「卡住」问题  ， 但这时会发现新的问题： 每行图片**右不对齐**

3. 思考可否通过对图片进行一些拉伸，让每行都对齐 ： Float 布局是无法实现， 这时候就想到了Flex的布局方式

4. Flex布局
 ```css
    .section{
        display: flex;
        flex-wrap:warp;
    }
    img{
        flex-grow:1;
    }
 ```

 5. 通过对img元素 添加 `object-fit:cover`属性，对图片做适当的裁剪，防止图片拉伸ugly（相当于 `background-clip`）

 6. 修改布局，添加cover弹层DOM 和 `hover` 交互效果
```css
    .toast{
            position: absolute;
            top:0;
            bottom: 0;
            left: 0;
            right: 0;
        }
        .toast:hover{
            background: rgba(77,77,77,0.3);
        }
        .toast:hover .title{
            height: 40px;
        }
        .title{
            background: rgba(0,0,0,0.5);
            position: absolute;
            bottom: 0;
            right: 0;
            left: 0;
            color:white;
            line-height: 40px;
            height: 0;
            text-align: center;
            padding: 0 10px;
        }
```

7. `transition`属性 为弹层添加「移入移出」动画效果

8. 为末尾添加div伪类， 让最后一行图片**左对齐， 并且不被拉伸**

9. 优化细节处理: 文字裁剪，防止长
```css
    title{
        overflow: hidden;
        white-space: nowrap;
        text-overflow: ellipsis;
    }
```

10. 借助GithubPages， 部署demo