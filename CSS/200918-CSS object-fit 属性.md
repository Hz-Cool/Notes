# CSS object-fit 属性

> 标签定义及使用说明

object-fit 属性指定元素的内容应该如何去适应指定容器的高度与宽度。

object-fit 一般用于 img 和 video 标签，一般可以对这些元素进行保留原始比例的剪切、缩放或者直接进行拉伸等。

注意: Internet Explorer/Edge 15 或更早版本的浏览器不支持 object-fit 属性。

```
object-fit: fill|contain|cover|scale-down|none|initial|inherit;
```

> 效果图

![](https://raw.githubusercontent.com/Hz-Cool/Notes/master/Images/200918-object-fit.png)

> 代码

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style>
      .fill {
        object-fit: fill;
      }

      .contain {
        object-fit: contain;
      }

      .cover {
        object-fit: cover;
      }

      .scale-down {
        object-fit: scale-down;
      }

      .none {
        object-fit: none;
      }

      .main {
        display: flex;
        justify-content: center;
      }
    </style>
  </head>
  <body>
    <div class="main">
      <div style="width: 300px;">
        <h1>object-fit 属性</h1>

        <h2>不使用 object-fit:</h2>
        <img
          src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png"
          alt="Paris"
          style="width:200px;height:400px;border:1px solid red"
        />

        <h2>object-fit: fill (默认):</h2>
        <img
          class="fill"
          src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png"
          alt="Paris"
          style="width:200px;height:400px;border:1px solid red"
        />

        <h2>object-fit: contain:</h2>
        <img
          class="contain"
          src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png"
          alt="Paris"
          style="width:200px;height:400px;border:1px solid red"
        />

        <h2>object-fit: cover:</h2>
        <img
          class="cover"
          src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png"
          alt="Paris"
          style="width:200px;height:400px;border:1px solid red"
        />

        <h2>object-fit: scale-down:</h2>
        <img
          class="scale-down"
          src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png"
          alt="Paris"
          style="width:200px;height:400px;border:1px solid red"
        />

        <h2>object-fit: none:</h2>
        <img
          class="none"
          src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png"
          alt="Paris"
          style="width:200px;height:400px;border:1px solid red"
        />

        <p>
          注意: Internet Explorer/Edge 15 或更早版本的浏览器不支持 object-fit
          属性。
        </p>
      </div>
    </div>
  </body>
</html>
```

> 属性值

| 值         | 描述                                                                                                        |
| ---------- | ----------------------------------------------------------------------------------------------------------- |
| fill       | 默认，不保证保持原有的比例，内容拉伸填充整个内容容器。                                                      |
| contain    | 保持原有尺寸比例。内容被缩放。                                                                              |
| cover      | 保持原有尺寸比例。但部分内容可能被剪切。                                                                    |
| none       | 保留原有元素内容的长度和宽度，也就是说内容不会被重置。                                                      |
| scale-down | 保持原有尺寸比例。内容的尺寸与 none 或 contain 中的一个相同，取决于它们两个之间谁得到的对象尺寸会更小一些。 |

> 参见

[菜鸟教程 object-fit](https://www.runoob.com/cssref/pr-object-fit.html)
