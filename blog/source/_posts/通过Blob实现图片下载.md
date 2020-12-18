---
title: 通过Blob实现图片下载
comments: true
date: 2020-08-06 20:50:25
tags: 
- 图片下载
- 文件下载
---

## 问题：通过iframe的方式下载图片的时候，不能够修改图片的名字

### 解决思路：

1. 因为图片地址是跨域的，所以先要转成 base64 数据流

2. 然后把 base64 转换成 blob对象

3. 然后判断浏览器的类型，选择不同的方式把 blob 文件流下载到本地

## 转换成base64的方法

```js
convertUrlToBase64(url) {
    return new Promise(function(resolve, reject) {
        var img = new Image();
        img.crossOrigin = "Anonymous";
        img.src = url;
        img.onload = function() {
            var canvas = document.createElement("canvas");
            canvas.width = img.width;
            canvas.height = img.height;
            var ctx = canvas.getContext("2d");
            ctx.drawImage(img, 0, 0, img.width, img.height);
            var ext = img.src
            .substring(img.src.lastIndexOf(".") + 1)
            .toLowerCase();
            var dataURL = canvas.toDataURL("image/" + ext);
            var base64 = {
                dataURL: dataURL,
                type: "image/" + ext,
                ext: ext
            };
            resolve(base64);
        };
    });
}
```

## 转换成 blob 对象

```js

convertBase64UrlToBlob(base64) {
    var parts = base64.dataURL.split(";base64,");
    var contentType = parts[0].split(":")[1];
    var raw = window.atob(parts[1]);
    var rawLength = raw.length;
    var uInt8Array = new Uint8Array(rawLength);
    for (var i = 0; i < rawLength; i++) {
        uInt8Array[i] = raw.charCodeAt(i);
    }
    return new Blob([uInt8Array], { type: contentType });
}
```

## 判断浏览器的类型

```js

myBrowser() {
    var userAgent = navigator.userAgent; //取得浏览器的userAgent字符串
    if (userAgent.indexOf("OPR") > -1) {
        return "Opera";
    } //判断是否Opera浏览器 OPR/43.0.2442.991
    if (userAgent.indexOf("Firefox") > -1) {
        return "FF";
    } //判断是否Firefox浏览器  Firefox/51.0
    if (userAgent.indexOf("Trident") > -1) {
        return "IE";
    } //判断是否IE浏览器  Trident/7.0; rv:11.0
    if (userAgent.indexOf("Edge") > -1) {
        return "Edge";
    } //判断是否Edge浏览器  Edge/14.14393
    if (userAgent.indexOf("Chrome") > -1) {
        return "Chrome";
    } // Chrome/56.0.2924.87
    if (userAgent.indexOf("Safari") > -1) {
        return "Safari";
    } //判断是否Safari浏览器 AppleWebKit/534.57.2 Version/5.1.7 Safari/534.57.2
}
```

## 把获取的地址传入上面的方法，然后判断浏览器的类型

```js

//图片格式和PDF
function downloadImage() {
    convertUrlToBase64(url).then(function(base64) {
        // 图片转为base64
        var blob = that.convertBase64UrlToBlob(base64); // 转为blob对象
        // 下载
        if (myBrowser() == "IE") {
            window.navigator.msSaveBlob(blob, name + ".jpg");
        } else if (myBrowser() == "FF") {
            window.location.href = url;
        } else {
            var a = document.createElement("a");
            a.download = name;
            a.href = URL.createObjectURL(blob);
            a.click();
        }
    })
}
```

