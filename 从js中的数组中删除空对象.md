## 从js中的数组中删除空对象

方法一

```js
var newArray = array.filter(value => Object.keys(value).length !== 0);
```

方法二

```js
JSON.stringify(array.filter(function(el) {
    // keep element if it's not an object, or if it's a non-empty object
    return typeof el != "object" || Array.isArray(el) || Object.keys(el).length > 0;
});
```

