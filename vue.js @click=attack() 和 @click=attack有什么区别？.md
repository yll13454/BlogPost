```js
@click="attack"
```

默认会带上事件对象$event参数

```js
@click="attack()"
```

不会带上$event, 但是可以传额外参数

```js
@click="attack"
@click="attack($event)"
```

这两者等同

```js
@click="attack($event, params)"
```

保留$event同时传入额外参数

实际上，两者除了参数的不同，在底层的绑定上也有不同。

当你用`@click="attack"` 绑定时，如果你在后续的代码里修改了attack，使其指向了另一个函数或者对其进行了其他的封装，你在点击时仍然是原来绑定的那个`attack`而不是新的`attack_new`。

这是因为Vue在处理两者时会不带()的会直接使用，而携带()的则在外层做了一个`() => attack()`的封装。

当然大部分情况下动态修改函数指向的情况不多。

