file - nums.js

```js
export let one = 1;
export let two = 2;
```

file - index.js

```js
import {one, two} from "./nums";

alert( `${one} and ${two}` ); // 1 and 2
```

file - index.js You can import all values at once as an object import * as obj, ex:

```js
import * as numbers from "./nums";
alert( `${numbers.one} and ${numbers.two}` ); // 1 and 2
```