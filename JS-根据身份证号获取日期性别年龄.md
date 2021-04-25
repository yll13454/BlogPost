```js
/**
* @description 解析身份证信息
* @param {String} idCard - 身份证号
* @param {Number} analyseType - 解析类型（birthDate-出生日期 sex-性别 age-年龄）
* @return {String}
*/
function getAnalysisIdCard(idCard = '', analyseType) {
    if (!analyseObj[analyseType]) {
        throw new Error('请传入正确的解析类型！')
    }
    const analyseObj = {
        "birthDate": (idCard) => {
            // 获取出生日期
            const birth = `${idCard.substring(6, 10)}-${idCard.substring(10, 12)}-${idCard.substring(12, 14)}`
            return birth;
        },
        "sex": (idCard) => {
            //获取性别
            const sex = parseInt(idCard.substr(16, 1)) % 2 === 1 ? "男" : "女" ;
            return sex;
        },
        "age": (idCard) => {
            //获取年龄(计算周岁，未过今年的生日则不加上一岁)
            const myDate = new Date(), 
            month = myDate.getMonth() + 1, 
            day = myDate.getDate();
            let age = myDate.getFullYear() - idCard.substring(6, 10) - 1;
            if (idCard.substring(10, 12) < month || idCard.substring(10, 12) == month && idCard.substring(12, 14) <= day) {
              age++;
            }
            return age;
        },
    }
    return analyseObj[analyseType](idCard)
  } 
```

