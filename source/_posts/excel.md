---
title: 网页实现excel下载
date: 2019-02-03
tags: js
---
在现在项目中遇到一个把表格作为excel下载的需求，之前一直绕到一个死胡同里，搜关键词都搜vue excel下载或json转excel下载,经大神提点，跟用什么框架等没有半毛钱关系，用下面的方法轻松实现啦！
<font size=1>**如有转载，请注明出处。**</font>
## excel
> 不喜勿喷~ 据大神说还有一种方法是用"/t"分割，我这里使用半角","分割的。

```javascript
  JSONToExcelConvertor() { // 导出Excel
    const json = this.countData.stat.prize;
    const fileName = this.countData.title;
    const arrData = typeof json !== 'object' ? JSON.parse(json) : json;
    /* eslint-disable */
    let excel = "奖品,收件人,手机号,省,市,区,收货地址\n";
    for (let i = 0; i < arrData.length; i += 1) {
      for (let j = 0; j < arrData[i].users.length; j += 1) {
        excel += `${arrData[i].name},
                  ${arrData[i].users[j].user.address.name},
                  ${arrData[i].users[j].user.address.phone},
                  ${arrData[i].users[j].user.address.city.split("-").join(",")},
                  ${arrData[i].users[j].user.address.address}\n`;
      }
    }
    excel = encodeURIComponent(excel);
    const link = document.createElement('a');
    link.href = `data:text/csv;charset=utf-8,\ufeff${excel}`;
    link.charset = 'gbk';
    link.download = `${fileName}.csv`;
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  },
```

## 请耐心等待，麦麦会不定期更新哒~
More info: [麦麦](https://github.com/maimai123)
