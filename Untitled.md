```js
h('div', [
              h(
                'el-button',
                {
                  props: {
                    icon: 'el-icon-edit-outline',
                    type: 'primary',
                    size: 'mini',
                    plain: true
                  },
                  style: {
                    marginRight: '5px'
                  },
                  on: {
                    click: (record, value) => {
                      console.log(value);
                      Vue.$router.push({
                        path: `testresultform/${record.id}`
                      });
                    }
                  }
                },
                '测试结果'
              ),
              h(
                'el-button',
                {
                  props: {
                    icon: 'el-icon-edit-outline',
                    type: 'primary',
                    size: 'mini',
                    plain: true
                  },
                  style: {
                    marginRight: '5px'
                  },
                  on: {
                    click: record => {
                      console.log(record.id);
                      Vue.$router.push({
                        path: `whitelistform/${record.id}`
                      });
                    }
                  }
                },
                '白名单'
              )
            ])
```



```js
  async handleSort(index) {
   const data = this.bannerList
   .filter((c, i) => [index, index - 1].includes(i))
    .reduce((r, c, i) => Object.assign(r, { [`id${i + 1}`]: c.id }), {});
   const res = await globalApi.sequenceBanner(data);
   if (res.code === 200) {
    Message.success(res.message);
   }
   await this.$emit('getbanner');
  }
```

