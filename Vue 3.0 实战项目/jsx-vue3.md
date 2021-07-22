### jsx

```js
import { withModifiers, defineComponent } from "vue";

const App = defineComponent({
  setup() {
    const count = ref(0);

    const inc = () => {
      count.value++;
    };

    return () => (
      <div onClick={withModifiers(inc, ["self"])}>{count.value}</div>
    );
  },
});
```

动态绑定:

```js
const placeholderText = "email";
const App = () => <input type="email" placeholder={placeholderText} />;
```

### 指令

v-show

```js
const App = {
  data() {
    return { visible: true };
  },
  render() {
    return <input v-show={this.visible} />;
  },
};
```

v-model

> 注意：如果想要使用 `arg`, 第二个参数需要为字符串

```html
<input v-model={val} />
<input v-model={[val, ["modifier"]]} />
<A v-model={[val, "argument", ["modifier"]]} />
```

v-models

> 注意: 你应该传递一个二维数组给 v-models。

```
<A v-models={[[foo], [bar, "bar"]]} />
<A
  v-models={[
    [foo, "foo"],
    [bar, "bar"],
  ]}
/>
<A
  v-models={[
    [foo, ["modifier"]],
    [bar, "bar", ["modifier"]],
  ]}
/>
```

自定义指令

```
const App = {
  directives: { custom: customDirective },
  setup() {
    return () => <a v-custom={[val, "arg", ["a", "b"]]} />;
  },
};
```

