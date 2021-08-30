# js-cookie使用方法

> js-cookie用来处理cookie相关的插件，非常简单好用，下面简单记录一下：

### 1、项目中引用：

```bash
npm install --save js-cookie
```

在typescript项目中需要添加类型定义

```bash
yarn add @types/js-cookie
```

### 2、js-cookie的使用：

安装好js-cookie插件后，在我们需要处理cookie的地方，简单的通过import引入就可以使用了

```js
import Cookies from 'js-cookie'
```





## 创建

```js
//创建简单的cookie
Cookies.set('name', 'value');
//创建有效期为7天的cookie
Cookies.set('name', 'value', { expires: 7 });
//为当前页创建有效期7天的cookie
Cookies.set('name', 'value', { expires: 7, path: '' });123456
```

## 取值

```js
Cookies.get('name'); // => 'value'
Cookies.get('nothing'); // => undefined
//获取所有cookie
Cookies.get(); // => { name: 'value' }1234
```

## 删除值

```js
Cookies.remove('name');

//如果值设置了路径，那么不能用简单的delete方法删除值，需要在delete时指定路径
Cookies.set('name', 'value', { path: '' });
Cookies.remove('name'); // 删除失败
Cookies.remove('name', { path: '' }); // 删除成功
//注意，删除不存在的cookie不会报错也不会有返回1234567
```

## 命名空间

如果担心不小心修改掉Cookies中的数据，可以用noConflict方法定义一个新的cookie。

```js
var Cookies2 = Cookies.noConflict();
Cookies2.set('name', 'value');12
```

## json相关

js-cookie允许你向cookie中存储json信息。

如果你通过set方法，传入Array或类似对象，而不是简单的string，那么js-cookie会将你传入的数据用JSON.stringify转换为string保存。

```js
Cookies.set('name', { foo: 'bar' });
Cookies.get('name'); // => '{"foo":"bar"}'
Cookies.get(); // => { name: '{"foo":"bar"}' }123
```

如果你用getJSON方法获取cookie，那么js-cookie会用JSON.parse解析string并返回。

```js
Cookies.getJSON('name'); // => { foo: 'bar' }
Cookies.getJSON(); // => { name: { foo: 'bar' } }12
```

## set方法支持的属性

1. expires
   定义有效期。如果传入Number，那么单位为天，你也可以传入一个Date对象，表示有效期至Date指定时间。默认情况下cookie有效期截止至用户退出浏览器。
2. path
   string，表示此cookie对哪个地址可见。默认为”/”。
3. domain
   string，表示此cookie对哪个域名可见。设置后cookie会对所有子域名可见。默认为对创建此cookie的域名和子域名可见。
4. secure
   true或false，表示cookie传输是否仅支持https。默认为不要求协议必须为https。

## 骚操作

------

1.read

> 通过withConverter方法可以覆写默认的decode实现，并返回一个新的cookie实例。所有与decode有关的get操作，如Cookies.get()或Cookies.get(‘name’)都会先执行此方法中的代码。

```js
document.cookie = 'escaped=%u5317';
document.cookie = 'default=%E5%8C%97';
var cookies = Cookies.withConverter(function (value, name) {
    if ( name === 'escaped' ) {
        return unescape(value);
    }
});
cookies.get('escaped'); // 北
cookies.get('default'); // 北
cookies.get(); // { escaped: '北', default: '北' }12345678910
```

------

2.write

> 通过withConverter方法也可以覆写默认的encode实现，并返回一个新的cookie实例。

```js
Cookies.withConverter({
    read: function (value, name) {
        // Read converter
    },
    write: function (value, name) {
        // Write converter
    }
});
```







### 3、js-cookie的増、查、删

##### 添加cookie

```js
// 创建一个名称为name，对应值为value的cookie，由于没有设置失效时间，默认失效时间为该网站关闭时
Cookies.set(name, value)

// 创建一个有效时间为7天的cookie
Cookies.set(name, value, { expires: 7 })

// 创建一个带有路径的cookie
Cookies.set(name, value, { path: '' })

// 创建一个value为对象的cookie
const obj = { name: 'ryan' }
Cookies.set('user', obj)
123456789101112
```

需要注意的是，通过Cookies.set(name, value)添加cookie时，即使添加时的value值类型为number，添加后获取到的value值的类型会被转换成string类型。

cookie添加后，所有的请求接口都会自动带上cookie值，如果没有设置cookie的失效时间，默认就是该网站关闭时cookie失效。

##### 获取cookie

```js
// 获取指定名称的cookie
Cookies.get(name) // value

// 获取value为对象的cookie
const obj = { name: 'ryan' }
Cookies.set('user', obj)
JSON.parse(Cookies.get('user'))

// 获取所有cookie
Cookies.get()
12345678910
```

获取cookie时，如果cookie中不存在该名称对应的记录，则会返回undefined。当value为对象时，获取的cookie需要通过JSON.parse()解析

##### 删除cookie

```js
// 删除指定名称的cookie
Cookies.remove(name) // value

// 删除带有路径的cookie
Cookies.set(name, value, { path: '' })
Cookies.remove(name, { path: '' })
123456
```

删除带有路径path的cookie时，不能通过简单的Cookies.remove(name)进行删除，需要带上路径

##### 参考文献：

[1] [Cookie的使用（js-cookie插件）](https://www.jianshu.com/p/6e1bacd35f59)
[2] [js-cookie](https://www.npmjs.com/package/js-cookie)





