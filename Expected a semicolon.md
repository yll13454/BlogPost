### 1

```js
 Expected a semicolon             @typescript-eslint/member-delimiter-style
```

######  .eslintrc.js

```json
"rules": {
    "@typescript-eslint/member-delimiter-style": ["error", {
      multiline: {
        delimiter: 'none',    // 'none' or 'semi' or 'comma'
        requireLast: true,
      },
      singleline: {
        delimiter: 'semi',    // 'semi' or 'comma'
        requireLast: false,
      },
    }]
}
```

### 2

```
Expected a space after the ':' @typescript-eslint/type-annotation-spacing
```

 .eslintrc.js

```
 "rules": {
    "@typescript-eslint/type-annotation-spacing": ["error"]
  }
```

