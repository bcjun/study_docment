## THML

+ 定义：超文本标记语言
  + 作用：开发网页

| 单标签              | 参数                                 | 功能   |
| ------------------- | ------------------------------------ | ------ |
| <img src="" alt=""> | src:表示路径  alt:图片加载失败后显示 | 图片   |
| <br/>               | 无                                   | 换行   |
| <hr>                | 无                                   | 分割线 |

| 基础双标签                             | 参数                    | 功能           |
| -------------------------------------- | ----------------------- | -------------- |
| <h1></h1>                              | 无                      | 标题           |
| <p></p>                                | 无                      | 段落           |
| <div></div>                            | 无                      | 块容器         |
| <a href=""></a>                        | 无                      | 超链接         |
| <span></span>                          |                         | 行内块         |
| **列标签**                             | **参数**                | **功能**       |
| <ul></ul>                              | 无                      | 无序列         |
| <ol></ol>                              | 无                      | 有序列         |
| <li></li>                              | 无                      | 配合上面的使用 |
| **表格标签**                           | **参数**                | **功能**       |
| <table></table>                        | 无                      | 声明一个表格   |
| <tr></tr>                              | 无                      | 表格中的一行   |
| <th></th>                              | 无                      | 首行单元格     |
| <td></td>                              | 无                      | 普通单元格     |
| **表单标签**                           | **参数**                | **功能**       |
| <form action="" method="">             | action:地址 method:方式 | 申明表单       |
| <label for="">                         |                         | 文字注释       |
| <input type="text" name="" value="">   | type有很多种类型        | 定义通用元素   |
| <textarea name="" cols="30" rows="10"> |                         | 多行文本输入框 |
| <select name="" id="">                 |                         | 下拉表单       |
| <option value="">                      |                         | 配合select使用 |
| PS:除input外其他都是双标签             |                         |                |
| **特殊标签或符号**                     | **参数**                | **功能**       |
| `&lt;`                                 | 无                      | <              |
| `&gt;`                                 | 无                      | >              |
| `&nbsp;`                               | 无                      | 空格           |
|                                        |                         |                |
|                                        |                         |                |



#### HTML例子

+ 列

~~~html
<!-- 无序 -->
<div>
    <ul id="NO_order_li">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>

</div>
<!-- 有序 -->
<div>
    <ol id="order_li">
        <li>一</li>
        <li>二</li>
        <li>三</li>
        <li>四</li>
        <li>五</li>
    </ol>
</div>
~~~



+ 表格

~~~html
<!-- 表格 -->
<table id="table">
    <tr>
        <th>年龄</th>
        <th>姓名</th>
        <th>升高</th>
        <th>体重</th>
        <th>爱好</th>
    </tr
    <tr>
        <td>24</td>
        <td>唐海</td>
        <td>170</td>
        <td>50kg</td>
        <td>很多</td>
    </tr>
</table>
~~~

+ 表单

~~~html
<!-- 表单 -->
<form action="" method="post">
    <div class="reg">
        <div class="reg-title">
            <h3>注册表单</h3>
        </div>
        <div class="list">
            <label>用户名：</label><input type="text" />
        </div>
        <div class="list">
            <label>密码：</label><input type="text" />
        </div>
        <div class="list">
            <label>确认密码：</label><input type="text" />
        </div>
        <div class="list">
            <label>邮箱：</label><input type="text" />
        </div>
        <div class="list">
            <label class="person">个人简介：</label><textarea></textarea>
        </div>
        <div class="check">
            <input type="checkbox" />
            <span>同意用户使用协议</span>
        </div>
        <div>
            <input type="submit" value="注册" class="reg-btn" />
        </div>
    </div>	
</form>

~~~



## CSS

+ **定义**：层叠样式表
+ **作用**：美化页面/控制页面的布局

+ **CSS导入方式:**
  + 行内式
  + 内嵌式
  + 外链式

+ **css选择器:**

  | 选择器种类 | 作用                                               |
  | ---------- | -------------------------------------------------- |
  | 标签选择器 | 通用设置                                           |
  | 类选择器   | 一个标签可以选择多个类，一个类也可以被多个标签使用 |
  | 层级选择器 | 选择有嵌套关系的标签，关系：父子，祖孙，           |
  | id选择器   | 选择唯一的标签                                     |
  | 组选择器   | 设置一组标签的公共配置                             |
  | 伪类选择器 | 给标签增加特效                                     |

+ **CSS常用样式**

| 布局样式        | 功能                                              |
| --------------- | ------------------------------------------------- |
| widh            | 设置元素(标签)的宽度                              |
| height          | 设置元素(标签)的高度                              |
| background      | 设置元素背景色或者背景图片                        |
| border          | 设置元素四周的边框                                |
|                 | border-top/border-left/border-right/border-bottom |
| padding         | 设置元素包含的内容和元素边框的距离                |
| margin          | 设置元素和外界的距离，也叫外边距                  |
| **文本样式**    | **功能**                                          |
| color           | 设置文字的颜色                                    |
| font-size       | 设置文字的大小                                    |
| font-family     | 设置文字的字体                                    |
| font-weight     | 设置文字是否加粗                                  |
| line-height     | 设置文字的行高                                    |
| text-decoration | 设置文字的下划线                                  |
| text-align      | 设置文字水平对齐方式                              |
| text-indent     | 设置文字首行缩进                                  |

#### CSS例子

~~~css
/* 标签选择器 */
div{color:red} 
/* 类选择器 */
.box{width:100px;height:100px;background:gold} 
/* 层级选择器 */
.con span{color:red}
.con .pink{color:pink}
/* id选择器 */
#box{color:red}
/* 组选择器 */
.box1,.box2,.box3{width:100px;height:100px}
/* 伪类选择器 */
.box1{width:100px;height:100px;background:gold;}
.box1:hover{width:300px;}
~~~

