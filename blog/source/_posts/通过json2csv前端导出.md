---
layout: 前端导出csv文件
title: 前端前端导出csv文件（通过json2csv前端导出）
date: 2017-08-15 20:36:14
tags: 
- csv表格下载
- 文件下载
---

## 第一步：安装依赖

```js
npm install json2csv  -s
```

## 第二步：代码实现

```js
rows: [
  {
    title: '序号',
    key: 'Ordinal',
    align: 'center'
  },
  {
    title: '产品编号',
    key: 'ProductNo',
    align: 'left'
  }
],
fields: ['title','key','align']
download(){
  try {
    const result = json2csv.parse(rows, {
      fields: fields,
      excelStrings: true
    });
    if (this.MyBrowserIsIE()) {
      // IE10以及Edge浏览器
      var BOM = "\uFEFF";
              // 文件转Blob格式
      var csvData = new Blob([BOM + result], { type: "text/csv" });
      navigator.msSaveBlob(csvData, `a.csv`);
    } else {
      let csvContent = "data:text/csv;charset=utf-8,\uFEFF" + result;
      // 非ie 浏览器
      this.createDownLoadClick(csvContent, `a.csv`);
    }
  } catch (err) {
    alert(err);
  }
},
//创建a标签下载
createDownLoadClick(content, fileName) {
  const link = document.createElement("a");
  link.href = encodeURI(content);
  link.download = fileName;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
},
// 判断是否IE浏览器
MyBrowserIsIE() {
  let isIE = false;
  if (
    navigator.userAgent.indexOf("compatible") > -1 &&
    navigator.userAgent.indexOf("MSIE") > -1
  ) {
    // ie浏览器
    isIE = true;
  }
  if (navigator.userAgent.indexOf("Trident") > -1) {
    // edge 浏览器
    isIE = true;
  }
  return isIE;
}
```

## 导出文件格式：

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-08-06_20-44-51.png %}

## 实际代码

```js
export function exportCsv(self, exportDataList) {
    getExportColumns(self.activeKey, self);
    const fields = [];
    self.exportColumns.forEach(t => {
        const temp = {
            value: t.dataIndex,
            label: t.title
        };
        fields.push(temp);
    });
    const result = json2csv.parse(exportDataList, {
        fields: fields,
        excelStrings: true
    });
    const fileName = self.exportFileName;
    const BOM = "\uFEFF";
    const csvData = new Blob([BOM + result], { type: "text/csv" });
    if (MyBrowserIsIE()) {
        // IE10以及Edge浏览器
        navigator.msSaveBlob(csvData, `${fileName}.csv`);
    } else {
        // 非ie 浏览器
        createDownLoadClick(csvData, `${fileName}.csv`);
    }
}

export function MyBrowserIsIE() {
    let isIE = false;
    if (
        navigator.userAgent.indexOf("compatible") > -1 &&
        navigator.userAgent.indexOf("MSIE") > -1
    ) {
        // ie浏览器
        isIE = true;
    }
    if (navigator.userAgent.indexOf("Trident") > -1) {
        // edge 浏览器
        isIE = true;
    }
    return isIE;
}

// 创建a标签下载
export function createDownLoadClick(content, fileName) {
    const link = document.createElement("a");
    link.href = URL.createObjectURL(content);
    link.download = fileName;
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
}
```

