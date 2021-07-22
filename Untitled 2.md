```js
computed: {
    id() {
      const id = this.$route.params.id;
      if (id === '0' || id === ':id') {
        return '';
      }
      return id;
    }
  },
```

![image-20201021192859402](media\image-20201021192859402.png)

- [ ] ![image-20201021193048034](media\image-20201021193048034.png)