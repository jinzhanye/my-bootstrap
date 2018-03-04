## Bootstrap 源码阅读笔记

源码版本Bootstrap3，主要对源码进行了一些注释，尤其是grid模块，在阅读过程中学习Less的使用。

## 目录结构

- less/bootstrap.less

	在该文件引入各个模块，最终通过编译bootstrap.less得到生产环境的css
	
- less/variables.less
	
	存在整个Bootsrap所有变量，定制化样式可以在这个文件做修改
	
- less/grid.less

	栅格系统，这是Bootstrap支持响应式的重点

- less/utilities.less

	工具样式，可以脱离Bootstap拿出来在平时开发中使用
	
- less/mixins/

	该文件夹存在各模块的mixin，用于代码复用

- less/normalize.less

	标准化css，这是一个专门将不同浏览器的默认css特性设置为统一效果的css库


	
## 实用技巧

可以看到Bootstrap一共只有黑、白、绿、蓝、黄、红这6个主要的颜色变量，其他颜色变量都是通过对原色加深或减浅生成的，大大提高了样式的可定制式与项目的可维护性。

````css
@gray-base: #000;
//lighten是less内置函数
//lighten： Increase the lightness of a color in the HSL color space by an absolute amount.
//通过改变黑色的亮度生成不同程度的灰
@gray-darker: lighten(@gray-base, 13.5%); // #222
@gray-dark: lighten(@gray-base, 20%); // #333
@gray: lighten(@gray-base, 33.5%); // #555
@gray-light: lighten(@gray-base, 46.7%); // #777
@gray-lighter: lighten(@gray-base, 93.5%); // #eee

@brand-primary: darken(#428bca, 6.5%); // #337ab7
@brand-success: #5cb85c;
@brand-info: #5bc0de;
@brand-warning: #f0ad4e;
@brand-danger: #d9534f;
````

通过加深颜色以表示状态的改变，减少颜色变量

````css
.bg-variant(@color) {
  ......
  .....
  a&:focus {
    //通过加深颜色以表示状态的改变，减少颜色变量
    background-color: darken(@color, 10%);
  }
}
````

自定义分割线，提高兼容性

````
hr {
  margin-top:@line-height-computed;
  margin-bottom:@line-height-computed;
  border: 0;
  border-top:1px solid @hr-border;
}
````

清浮动

````css
.clearfix() {
  &::before,
  &::after {
    content: " "; 
    display: table;
  }
  &::after {
    clear: both;
  }
}
````

利用递归生成样式

````
//生成以下class
//.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1,
//.col-xs-2, .col-sm-2, .col-md-2, .col-lg-2,
//.col-xs-3, .col-sm-3, .col-md-3, .col-lg-3,
//.......{
//      position: relative;
//      min-height: 1px;
//      padding-left: 15px;
//      padding-right: 15px;
//}
.make-grid-columns() {
  // Common styles for all sizes of grid columns, widths 1-12
  .col(@index) {
    //'~'号搭配字符串 用于避免编译，详情 http://www.bootcss.com/p/lesscss/
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), @item)
  }

  // =<就是小于或等于，好逆人类的符号 --__--||
  // @grid-columns默认是12
  // less没有for循环，但可以递归调用循环拼接字符串
  // .col-xs-1, .col-sm-1, .col-md-1, .col-lg-1,
  // .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2,
  // .col-xs-3, .col-sm-3, .col-md-3, .col-lg-3,
  // ..........
  //  直到index为12，调用.col(@index, @list) when (@index > @grid-columns) 输出样式
  .col(@index, @list) when (@index =< @grid-columns) {
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
  }

  .col(@index, @list) when (@index > @grid-columns) {
    @{list} {
      position: relative;
      // Prevent columns from collapsing when empty
      // 防止当内容为空时盒子崩塌
      min-height: 1px;
      // Inner gutter via padding
      // 利用padding充当列之间的内间隔
      padding-left: ceil((@grid-gutter-width / 2));
      padding-right: floor((@grid-gutter-width / 2));
    }
  }

  .col(1)
}
````