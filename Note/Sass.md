# sass

## 什么是sass

- Sass 扩展了 CSS3，增加了规则、变量、混入、选择器、继承等等特性。Sass 生成良好格式化的 CSS 代码，易于组织和维护
- SASS是对CSS3(层叠样式表)的语法的一种扩充，它可以使用巢状、混入、选择子继承等功能，可以更有效有弹性的写出Stylesheet。Sass最后还是会编译出合法的CSS让浏览可以使用，也就是说它本身的语法并不太容易让浏览器识别(虽然它和CSS的语法非常的像，几乎一样)，因为它不是标准的CSS格式，在它的语法内部可以使用动态变量等，所以它更像一种极简单的动态语言
- 工程越大，css文件越大，重复代码越来越多，会变得难以维护，借助Sass可以提高写css的效率

## 安装

- `npm install -g sass`

## 编译.scss文件为.css文件

- `sass input.scss ouput.css`
- `sass --watch input.scss:ouput.css`

## 基础用法与功能

1. 变量

   - 定义变量后，在选择器里可以直接引用

   - 大括号内为局部变量，括号外为局部变量

   - 变量名使用中划线或下划线都是指向同一变量

   - 后定义的变量声明会被忽略，但赋值会被执行，这一点和ES5中var声明变量是一样的

   - ```scss
     $border-color:#aaa; //声明变量
     $border_color:#ccc;
     .container {
         $border-width:1px;
         border:$border-width solid $border-color; //使用变量
     }
     编译后的CSS
     .container {
         border:1px solid #ccc; //使用变量
     }
     ```

2. 嵌套	

   - 父选择器里可以嵌套子选择器

   - 父元素，使用&表示

   - ```scss
     // 选择器嵌套
     .container ul {
         border:1px solid #aaa;
         list-style:none;
         
         li {
             float:left;
         }
         
         li>a {
             display:inline-block;
             padding:6px 12px;
         }
         
         &:after {
             display:block;
             content:"";
             clear:both;
         }
     }
     ```
     
   - 子代选择器可以写在外层选择器右边（如下述例子）也可以写在内层选择器左边

   - 写在外层选择器右边时要特别注意，他会作用于所有嵌套的选择器上

   - ```scss
     li{
         >a{
             display:inline-block;
             padding:6px 12px;
         }
     }
     
     li >{ 
         a {
             display:inline-block;
             padding:6px 12px;
         }
     }
     ```

   - 一个属性以分号结尾时则判断为一个属性

   - 以大括号结尾时则判断为一个嵌套属性

   - 将外部的属性以及内部的属性通过中划线连接起来形成一个新的属性

   - ```scss
     // 属性嵌套
     div{
         border: 1px solid $color{
             left: 0;
             top: 0;
         };
     }
     ```

3. mixin 混合

   - 相当于预先写好了一组样式，其它地方通过（@include 名字）调用

   - ```scss
     @mixin 名字（参数1，参数2，...）
     {
     ........样式.......
     
     }
     
     ```

   - ```scss
     @mixin mix($arg1, $arg2) {
         color: $arg1;
         a{
             font-size: $arg2
         }
     }
     
     div{
         @include mix(red, 12px);
         // @include mix($arg2:12px, $arg1:red);
     }
     ```

4. 继承/扩展

   - 一个选择器可以继承另一个选择器的全部样式

   - ```scss
     .one{
         color: #000;
     }
     .one a{
         font-size: 10px;
     }
     .two{
         @extend .one;
         background-color: #fff;
     }
     
     // .two类里继承了.one类的全部样式 （@extend 名字）； 还不止.one的，跟.one相关的都继承了
     // 相当于将one替换为two
     ```

5. @import

   - 引入一个.scss后缀的文件作为自己的一部分

   - 被引入的那个文件并不会转换成.css格式的

   - 变量默认值：在引入的文件中不存在已有变量时起作用

   - ```scss
     /*App1.scss*/
     $border-color:#aaa; //声明变量
     @import App2.scss;  //引入另一个SCSS文件
     .container {
         border:1px solid $border-color; //使用变量
     }
     /*App2.scss*/
     $border-color:#ccc !default; //声明变量
     
     /*生成的css文件*/
     .container {
         border:1px solid #aaa; //使用变量
     }
     
     ```

6. 计算功能

   - SASS允许在代码中使用算式

   - ```scss
     div{
         width: 10px*2;
     }
     ```

7. 颜色函数

   - SASS提供了一些内置的颜色函数，以便生成系列颜色

   - ```scss
     // hsl（色相，饱和度，明度，不透明度）
     body{   
        background-color: hsl(5, 20%, 20%, 0.5);
     } 
     ```

   - ```scss
     // 用来调整色相： adjust-hue(颜色，调整的度数)
     body{   
        background-color: adjust-hue(hsl(0,100,50%), 100deg);
     } 
     ```

   - ```scss
     // 用来调整颜色明度：lighten让颜色更亮，darken让颜色更暗
     body{   
        background-color: lighten(rgb(228, 145, 145),50%);
        color: darken(rgb(228, 145, 145),50%);
     } 
     ```

   - ```scss
     // 用来调整颜色纯度 saturate让颜色更纯 ，desaturate让颜色不纯 saturate（颜色，百分比）
     p {
       color: hsl($hue: 0, $saturation: 100%, $lightness: 50%);
     }
     ```

8.  Interpolation

   - 把一个值插入到另一个值

   - ```scss
     $color: color;
     div{
         p{
             #{$color}: red;
         }
     }
     ```

9. if 判断

   - ```scss
     @if 判断条件 {
     .......执行语句...
     } @else {
       ...else有就写没就不写....
     }
     ```

   - ```scss
     $condition: false;
     div{
         @if $condition {
             color: red;
         }@else{
             color: blue;
         }
     }
     ```

10. for循环

    - ```scss
      结束值不执行：
      @for 变量 from 开始值 through 结束值 {
           ......
      }
      结束值也执行：
      @for 变量 from 开始值 to 结束值 {
           ......
      }
      ```

    - ```scss
      @for $index from 1 to 3 {
          .div#{$index}{
              height: 100px * $index;
          }
      }
      ```

11. 列表循环

    - 能循环一遍一个列表的值，列表相当于数组

    - ```scss
      @each 变量 in 列表{
      ...
      }
      ```

    - ```scss
      $color_list: red, blue, green;
      @each $item in $color_list {
          .div#{$item}{
              color: $item;
          }
      }
      ```

12. while循环

    - ```scss
      @while 条件 {
         ...
      }
      ```

    - ```scss
      $count: 0;
      @while $count<10 {
          div#{$count}{
              height: 100px * $count;
          }
          $count: $count + 1;
      }
      ```

13. 自定义函数 function

    - 自己定义的函数可以调用

    - ```scss
      @function 名字(参数1，参数2，..){
      ....
      }
      ```

    - ```scss
      @function addHeight($ori){
          @return $ori * 2
      };
      div {
          height: addHeight(100px);
      }
      ```

## 参考

- [Sass总结笔记 基础入门（超级直观细节）](https://juejin.cn/post/6971458017267187719)
- [scss快速入门](https://juejin.cn/post/6844903859010158600#heading-3)

