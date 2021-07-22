### 选项

类型：`Object | Null`
默认：

```
{ 
    rootValue : 16 , 
    unitPrecision : 5 , 
    propList : [ 'font' ,  'font-size' ,  'line-height' ,  'letter-spacing' ] , 
    selectorBlackList : [ ] , 
    replace : true , 
    mediaQuery : false , 
    minPixelValue : 0 ，
    exclude：/node_modules/i 
}
```

- `rootValue`(Number | Function) 代表根元素字体大小或根据[`input`](https://api.postcss.org/Input.html)参数返回根元素字体大小

- `unitPrecision` （数字）允许 REM 单位增长到的十进制数字。

- ```
  propList
  ```

   （数组）可以从 px 更改为 rem 的属性。

  - 值需要完全匹配。
  - 使用通配符`*`启用所有属性。例子：`['*']`
  - `*`在单词的开头或结尾使用。（`['*position*']`会匹配`background-position-y`）
  - 使用`!`不匹配的属性。例子：`['*', '!letter-spacing']`
  - 将“not”前缀与其他前缀组合在一起。例子：`['*', '!font*']`

- ```
  selectorBlackList
  ```

   （数组）要忽略并保留为 px 的选择器。

  - 如果值是字符串，它会检查选择器是否包含字符串。
    - `['body']` 会匹配 `.body-class`
  - 如果 value 是 regexp，它会检查选择器是否与 regexp 匹配。
    - `[/^body$/]`将匹配`body`但不匹配`.body`

- `replace` （布尔值）替换包含 rems 的规则而不是添加回退。

- `mediaQuery` （布尔值）允许在媒体查询中转换 px。

- `minPixelValue` （数字）设置要替换的最小像素值。

- ```
  exclude
  ```

   （字符串，正则表达式，函数）要忽略并保留为 px 的文件路径。

  - 如果值为字符串，则检查文件路径是否包含该字符串。
    - `'exclude'` 会匹配 `\project\postcss-pxtorem\exclude\path`
  - 如果 value 是 regexp，它会检查文件路径是否与 regexp 匹配。
    - `/exclude/i` 会匹配 `\project\postcss-pxtorem\exclude\path`
  - 如果值为函数，则可以使用 exclude 函数返回 true，文件将被忽略。
    - 回调将文件路径作为参数传递，它应该返回一个布尔结果。
    - `function (file) { return file.indexOf('exclude') !== -1; }`

### 与 gulp-postcss 和 autoprefixer 一起使用

```
var gulp = require('gulp');
var postcss = require('gulp-postcss');
var autoprefixer = require('autoprefixer');
var pxtorem = require('postcss-pxtorem');

gulp.task('css', function () {

    var processors = [
        autoprefixer({
            browsers: 'last 1 version'
        }),
        pxtorem({
            replace: false
        })
    ];

    return gulp.src(['build/css/**/*.css'])
        .pipe(postcss(processors))
        .pipe(gulp.dest('build/css'));
});
```

### 关于忽略属性的消息

目前，忽略单个属性的最简单方法是在像素单元声明中使用大写字母。

```
// `px`被转换到`rem`
.convert {
    font-size: 16px; // converted to 1rem
}

// `Px`或`PX`由`postcss忽略- PX至rem`而是由浏览器还是接受
.ignore {
    border: 1Px solid; // ignored
    border-width: 2PX; // ignored
}
```