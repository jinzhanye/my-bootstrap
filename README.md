- 私有属性写前面，标准属性写最后
- ::after表示法是在CSS 3中引入的，::符号是用来区分伪类和伪元素的。支持CSS3的浏览器同时也都支持CSS2中引入的表示法:after。


https://blog.getbootstrap.com/2013/08/19/bootstrap-3-released/

- Media query bubbling and nested media queries 

container中响应式布局用到

````css
媒体查询同样可以嵌套在选择器中。选择器将被复制到媒体查询体内：

.screencolor{
  @media screen {
    color: green;
    @media (min-width:768px) {
    color: red;
    }
    }
  @media tv {
    color: black;
  }
}

//输出:

@media screen {
  .screencolor {
    color: green;
  }
}
@media screen and (min-width: 768px) {
  .screencolor {
    color: red;
  }
}
@media tv {
  .screencolor {
    color: black;
  }
}
````

通过加深颜色以表示改变状态

````css
// Contextual backgrounds

.bg-variant(@color) {
  background-color: @color;
  a&:hover,
  a&:focus {
    background-color: darken(@color, 10%);
  }
}
````
