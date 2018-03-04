less 嵌套

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
[Bootstrap3新特性](https://blog.getbootstrap.com/2013/08/19/bootstrap-3-released/)
