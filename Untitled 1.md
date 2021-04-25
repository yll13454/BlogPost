```js
async onSubmit() {
      await this.$refs['upImageForm'].validate();
      if (this.formData.id && Number(this.formData.id)) {
        const res = await globalApi.updateBanner(this.formData);
        if (res.code !== 200) {
          this.$message.error(res.message);
          return;
        } else {
          this.$message.success('更新Banner成功！');
          await this.$emit('getbanner');
        }
      } else {
        this.formData.type = this.type;
        const res = await globalApi.addBanner(this.formData);
        if (res.code !== 200) {
          this.$message.error(res.message);
          return;
        } else {
          this.$message.success('新增Banner成功！');
          await this.$emit('getbanner');
        }
      }
      this.visible = false;
    }
```

this.$refs['form'].validate

```
__searchList: (queryParams = {}) => {
      const checkedData = this.checkedData;
      return new Promise(resolve => {
        return courseApi.getCourseList(queryParams).then(res => {
          res.data.map(cur => {
            if (checkedData.some(item => item.id === cur.id)) {
              cur.checked = true;
            } else {
              cur.checked = false;
            }
            return cur;
          });
          resolve(res);
        });
```

