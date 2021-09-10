用来对比字符串是否一致，一般是判断已有文本是否被再次编辑。

```cpp
function hashVal(string){
	var hash = 0, i, chr;
	if (string.length === 0) return hash;
	for (i = 0; i < string.length; i++) {
	chr = string.charCodeAt(i);
	hash = ((hash << 5) - hash) + chr;
	hash |= 0; // Convert to 32bit integer
	}
    return hash;
}
let string = '你好有缘人';
console.log(hashVal(string)); // -2000030674
```

