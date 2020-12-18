---
title: 前端通过Blob实现文件下载
comments: true
date: 2019-08-06 19:59:09
tags: 
- Blob
- 文件下载
---

最近遇到一个需求，需要将页面中的配置信息下载下来供用户方便使用，以前这个场景的需求有时候会放到后端处理，然后给返回一个下载链接。其实并不需要这么麻烦，这样既增大了服务器的负载，也让用户产生了没有必要的网络请求，现在前端也是可以直接通过Blob对象进行前端文件下载了，下面简单记录下相关实现

## Blob对象简要介绍

 `Blob` 对象表示一个不可变、原始数据的类文件对象。`Blob` 表示的不一定是`JavaScript`原生格式的数据。`File` 接口基于`Blob`，继承了 `Blob` 的功能并将其扩展使其支持用户系统上的文件 

### 语法

```js
const aBlob = new Blob( array, options ); 
```

###  **参数说明** 

- array 是一个由ArrayBuffer, ArrayBufferView, Blob, DOMString 等对象构成的 Array ，或者其他类似对象的混合体，它将会被放进 Blob。DOMStrings会被编码为UTF-8。
- options 是一个可选的BlobPropertyBag字典，它可能会指定如下两个属性：
  - type，默认值为 ""，它代表了将会被放入到blob中的数组内容的MIME类型。
  - endings，默认值为"transparent"，用于指定包含行结束符\n的字符串如何被写入。 它是以下两个值中的一个： "native"，代表行结束符会被更改为适合宿主操作系统文件系统的换行符，或者 "transparent"，代表会保持blob中保存的结束符不变

### 示例

```js
const debug = {hello: "world"};
const blob = new Blob([JSON.stringify(debug, null, 2)],{type : 'application/json'});
```

## URL.createObjectURL() 与 URL.revokeObjectURL()介绍

URL.createObjectURL() 静态方法会创建一个 DOMString，其中包含一个表示参数中给出的对象的URL。这个 URL 的生命周期和创建它的窗口中的 document 绑定。这个新的URL 对象表示指定的 File 对象或 Blob 对象。相当于这个方法创建了一个传入对象的内存引用地址

### createObjectURL语法

```js
objectURL = URL.createObjectURL(object);
```

#### 参数说明

-  object 是用于创建 URL 的 File 对象、Blob 对象或者 MediaSource 对象。

#### 返回值

-  一个可以引用到指定对象的`DOMString` 

`URL.revokeObjectURL()` 静态方法用来释放一个之前已经存在的、通过调用 `URL.createObjectURL()` 创建的 `URL` 对象。当你结束使用某个 `URL` 对象之后，应该通过调用这个方法来让浏览器知道不用在内存中继续保留对这个文件的引用了。

你可以在 `sourceopen` 被处理之后的任何时候调用 `revokeObjectURL()`。这是因为 `createObjectURL()` 仅仅意味着将一个媒体元素的 `src` 属性关联到一个 `MediaSource` 对象上去。调用`revokeObjectURL()` 使这个潜在的对象回到原来的地方，允许平台在合适的时机进行**垃圾收集**。

### revokeObjectURL语法

```js
window.URL.revokeObjectURL(objectURL);
```

####  **参数说明** 

-  objectURL 是一个 DOMString，表示通过调用 `URL.createObjectURL()` 方法产生的 URL 对象 

####  **内存管理** 

-  在每次调用`createObjectURL()` 方法时，都会创建一个新的 URL 对象，即使你已经用相同的对象作为参数创建过。当不再需要这些 URL 对象时，每个对象必须通过调用 `URL.revokeObjectURL()` 方法来释放。浏览器会在文档退出的时候自动释放它们，但是为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们。 

## 实际运用

 比如在某后台管理中希望将用户的几个配置信息导入到一个`json`文件当中供用户下载下来 

```js
const config = {
  name: 'lsqy',
  password: 'yourpassword',
  ak: 'XXXXXXXXXX',
  sk: 'XXXXXXXXXX'
}

const blobContent = new Blob(
  [JSON.stringify(config, null, 2)],
  {type : 'application/json'}
);

const blobUrl = window.URL.createObjectURL(blobContent)

downloadFileByBlob(blobUrl, 'config.json')

function downloadFileByBlob(blobUrl, filename) {
  const eleLink = document.createElement('a')
  eleLink.download = filename
  eleLink.style.display = 'none'
  eleLink.href = blobUrl
  // 触发点击
  document.body.appendChild(eleLink)
  eleLink.click()
  // 然后移除
  document.body.removeChild(eleLink)
}
```

执行上面的代码，我们可以得到一个`config.json`的文件，可以看到，其实很简单就实现了这个场景需求，当然这里是下载的`json`文件,下载其他的文件也是一样的道理，只是需要得到相应文件的`blob`数据，再结合相应的[MIME](https://www.w3school.com.cn/media/media_mimeref.asp)类型即可;

兼容性方面目前主流浏览器都已支持，ie10以及以上也支持。

另外`Blob`结合`URL. createObjectURL()`与`URL.revokeObjectURL()`还可以用在预览图片、预览PDF、视频链接防盗等多种场景中，大家可以发挥自己的想象力来进行实现

## Mime类型

### 按照内容类型排列的 Mime 类型列表

| 类型/子类型                             | 扩展名  |
| :-------------------------------------- | :------ |
| application/envoy                       | evy     |
| application/fractals                    | fif     |
| application/futuresplash                | spl     |
| application/hta                         | hta     |
| application/internet-property-stream    | acx     |
| application/mac-binhex40                | hqx     |
| application/msword                      | doc     |
| application/msword                      | dot     |
| application/octet-stream                | *       |
| application/octet-stream                | bin     |
| application/octet-stream                | class   |
| application/octet-stream                | dms     |
| application/octet-stream                | exe     |
| application/octet-stream                | lha     |
| application/octet-stream                | lzh     |
| application/oda                         | oda     |
| application/olescript                   | axs     |
| application/pdf                         | pdf     |
| application/pics-rules                  | prf     |
| application/pkcs10                      | p10     |
| application/pkix-crl                    | crl     |
| application/postscript                  | ai      |
| application/postscript                  | eps     |
| application/postscript                  | ps      |
| application/rtf                         | rtf     |
| application/set-payment-initiation      | setpay  |
| application/set-registration-initiation | setreg  |
| application/vnd.ms-excel                | xla     |
| application/vnd.ms-excel                | xlc     |
| application/vnd.ms-excel                | xlm     |
| application/vnd.ms-excel                | xls     |
| application/vnd.ms-excel                | xlt     |
| application/vnd.ms-excel                | xlw     |
| application/vnd.ms-outlook              | msg     |
| application/vnd.ms-pkicertstore         | sst     |
| application/vnd.ms-pkiseccat            | cat     |
| application/vnd.ms-pkistl               | stl     |
| application/vnd.ms-powerpoint           | pot     |
| application/vnd.ms-powerpoint           | pps     |
| application/vnd.ms-powerpoint           | ppt     |
| application/vnd.ms-project              | mpp     |
| application/vnd.ms-works                | wcm     |
| application/vnd.ms-works                | wdb     |
| application/vnd.ms-works                | wks     |
| application/vnd.ms-works                | wps     |
| application/winhlp                      | hlp     |
| application/x-bcpio                     | bcpio   |
| application/x-cdf                       | cdf     |
| application/x-compress                  | z       |
| application/x-compressed                | tgz     |
| application/x-cpio                      | cpio    |
| application/x-csh                       | csh     |
| application/x-director                  | dcr     |
| application/x-director                  | dir     |
| application/x-director                  | dxr     |
| application/x-dvi                       | dvi     |
| application/x-gtar                      | gtar    |
| application/x-gzip                      | gz      |
| application/x-hdf                       | hdf     |
| application/x-internet-signup           | ins     |
| application/x-internet-signup           | isp     |
| application/x-iphone                    | iii     |
| application/x-javascript                | js      |
| application/x-latex                     | latex   |
| application/x-msaccess                  | mdb     |
| application/x-mscardfile                | crd     |
| application/x-msclip                    | clp     |
| application/x-msdownload                | dll     |
| application/x-msmediaview               | m13     |
| application/x-msmediaview               | m14     |
| application/x-msmediaview               | mvb     |
| application/x-msmetafile                | wmf     |
| application/x-msmoney                   | mny     |
| application/x-mspublisher               | pub     |
| application/x-msschedule                | scd     |
| application/x-msterminal                | trm     |
| application/x-mswrite                   | wri     |
| application/x-netcdf                    | cdf     |
| application/x-netcdf                    | nc      |
| application/x-perfmon                   | pma     |
| application/x-perfmon                   | pmc     |
| application/x-perfmon                   | pml     |
| application/x-perfmon                   | pmr     |
| application/x-perfmon                   | pmw     |
| application/x-pkcs12                    | p12     |
| application/x-pkcs12                    | pfx     |
| application/x-pkcs7-certificates        | p7b     |
| application/x-pkcs7-certificates        | spc     |
| application/x-pkcs7-certreqresp         | p7r     |
| application/x-pkcs7-mime                | p7c     |
| application/x-pkcs7-mime                | p7m     |
| application/x-pkcs7-signature           | p7s     |
| application/x-sh                        | sh      |
| application/x-shar                      | shar    |
| application/x-shockwave-flash           | swf     |
| application/x-stuffit                   | sit     |
| application/x-sv4cpio                   | sv4cpio |
| application/x-sv4crc                    | sv4crc  |
| application/x-tar                       | tar     |
| application/x-tcl                       | tcl     |
| application/x-tex                       | tex     |
| application/x-texinfo                   | texi    |
| application/x-texinfo                   | texinfo |
| application/x-troff                     | roff    |
| application/x-troff                     | t       |
| application/x-troff                     | tr      |
| application/x-troff-man                 | man     |
| application/x-troff-me                  | me      |
| application/x-troff-ms                  | ms      |
| application/x-ustar                     | ustar   |
| application/x-wais-source               | src     |
| application/x-x509-ca-cert              | cer     |
| application/x-x509-ca-cert              | crt     |
| application/x-x509-ca-cert              | der     |
| application/ynd.ms-pkipko               | pko     |
| application/zip                         | zip     |
| audio/basic                             | au      |
| audio/basic                             | snd     |
| audio/mid                               | mid     |
| audio/mid                               | rmi     |
| audio/mpeg                              | mp3     |
| audio/x-aiff                            | aif     |
| audio/x-aiff                            | aifc    |
| audio/x-aiff                            | aiff    |
| audio/x-mpegurl                         | m3u     |
| audio/x-pn-realaudio                    | ra      |
| audio/x-pn-realaudio                    | ram     |
| audio/x-wav                             | wav     |
| image/bmp                               | bmp     |
| image/cis-cod                           | cod     |
| image/gif                               | gif     |
| image/ief                               | ief     |
| image/jpeg                              | jpe     |
| image/jpeg                              | jpeg    |
| image/jpeg                              | jpg     |
| image/pipeg                             | jfif    |
| image/svg+xml                           | svg     |
| image/tiff                              | tif     |
| image/tiff                              | tiff    |
| image/x-cmu-raster                      | ras     |
| image/x-cmx                             | cmx     |
| image/x-icon                            | ico     |
| image/x-portable-anymap                 | pnm     |
| image/x-portable-bitmap                 | pbm     |
| image/x-portable-graymap                | pgm     |
| image/x-portable-pixmap                 | ppm     |
| image/x-rgb                             | rgb     |
| image/x-xbitmap                         | xbm     |
| image/x-xpixmap                         | xpm     |
| image/x-xwindowdump                     | xwd     |
| message/rfc822                          | mht     |
| message/rfc822                          | mhtml   |
| message/rfc822                          | nws     |
| text/css                                | css     |
| text/h323                               | 323     |
| text/html                               | htm     |
| text/html                               | html    |
| text/html                               | stm     |
| text/iuls                               | uls     |
| text/plain                              | bas     |
| text/plain                              | c       |
| text/plain                              | h       |
| text/plain                              | txt     |
| text/richtext                           | rtx     |
| text/scriptlet                          | sct     |
| text/tab-separated-values               | tsv     |
| text/webviewhtml                        | htt     |
| text/x-component                        | htc     |
| text/x-setext                           | etx     |
| text/x-vcard                            | vcf     |
| video/mpeg                              | mp2     |
| video/mpeg                              | mpa     |
| video/mpeg                              | mpe     |
| video/mpeg                              | mpeg    |
| video/mpeg                              | mpg     |
| video/mpeg                              | mpv2    |
| video/quicktime                         | mov     |
| video/quicktime                         | qt      |
| video/x-la-asf                          | lsf     |
| video/x-la-asf                          | lsx     |
| video/x-ms-asf                          | asf     |
| video/x-ms-asf                          | asr     |
| video/x-ms-asf                          | asx     |
| video/x-msvideo                         | avi     |
| video/x-sgi-movie                       | movie   |
| x-world/x-vrml                          | flr     |
| x-world/x-vrml                          | vrml    |
| x-world/x-vrml                          | wrl     |
| x-world/x-vrml                          | wrz     |
| x-world/x-vrml                          | xaf     |
| x-world/x-vrml                          | xof     |

### 按照文件扩展名排列的 Mime 类型列表

| 扩展名  | 类型/子类型                             |
| :------ | :-------------------------------------- |
|         | application/octet-stream                |
| 323     | text/h323                               |
| acx     | application/internet-property-stream    |
| ai      | application/postscript                  |
| aif     | audio/x-aiff                            |
| aifc    | audio/x-aiff                            |
| aiff    | audio/x-aiff                            |
| asf     | video/x-ms-asf                          |
| asr     | video/x-ms-asf                          |
| asx     | video/x-ms-asf                          |
| au      | audio/basic                             |
| avi     | video/x-msvideo                         |
| axs     | application/olescript                   |
| bas     | text/plain                              |
| bcpio   | application/x-bcpio                     |
| bin     | application/octet-stream                |
| bmp     | image/bmp                               |
| c       | text/plain                              |
| cat     | application/vnd.ms-pkiseccat            |
| cdf     | application/x-cdf                       |
| cer     | application/x-x509-ca-cert              |
| class   | application/octet-stream                |
| clp     | application/x-msclip                    |
| cmx     | image/x-cmx                             |
| cod     | image/cis-cod                           |
| cpio    | application/x-cpio                      |
| crd     | application/x-mscardfile                |
| crl     | application/pkix-crl                    |
| crt     | application/x-x509-ca-cert              |
| csh     | application/x-csh                       |
| css     | text/css                                |
| dcr     | application/x-director                  |
| der     | application/x-x509-ca-cert              |
| dir     | application/x-director                  |
| dll     | application/x-msdownload                |
| dms     | application/octet-stream                |
| doc     | application/msword                      |
| dot     | application/msword                      |
| dvi     | application/x-dvi                       |
| dxr     | application/x-director                  |
| eps     | application/postscript                  |
| etx     | text/x-setext                           |
| evy     | application/envoy                       |
| exe     | application/octet-stream                |
| fif     | application/fractals                    |
| flr     | x-world/x-vrml                          |
| gif     | image/gif                               |
| gtar    | application/x-gtar                      |
| gz      | application/x-gzip                      |
| h       | text/plain                              |
| hdf     | application/x-hdf                       |
| hlp     | application/winhlp                      |
| hqx     | application/mac-binhex40                |
| hta     | application/hta                         |
| htc     | text/x-component                        |
| htm     | text/html                               |
| html    | text/html                               |
| htt     | text/webviewhtml                        |
| ico     | image/x-icon                            |
| ief     | image/ief                               |
| iii     | application/x-iphone                    |
| ins     | application/x-internet-signup           |
| isp     | application/x-internet-signup           |
| jfif    | image/pipeg                             |
| jpe     | image/jpeg                              |
| jpeg    | image/jpeg                              |
| jpg     | image/jpeg                              |
| js      | application/x-javascript                |
| latex   | application/x-latex                     |
| lha     | application/octet-stream                |
| lsf     | video/x-la-asf                          |
| lsx     | video/x-la-asf                          |
| lzh     | application/octet-stream                |
| m13     | application/x-msmediaview               |
| m14     | application/x-msmediaview               |
| m3u     | audio/x-mpegurl                         |
| man     | application/x-troff-man                 |
| mdb     | application/x-msaccess                  |
| me      | application/x-troff-me                  |
| mht     | message/rfc822                          |
| mhtml   | message/rfc822                          |
| mid     | audio/mid                               |
| mny     | application/x-msmoney                   |
| mov     | video/quicktime                         |
| movie   | video/x-sgi-movie                       |
| mp2     | video/mpeg                              |
| mp3     | audio/mpeg                              |
| mpa     | video/mpeg                              |
| mpe     | video/mpeg                              |
| mpeg    | video/mpeg                              |
| mpg     | video/mpeg                              |
| mpp     | application/vnd.ms-project              |
| mpv2    | video/mpeg                              |
| ms      | application/x-troff-ms                  |
| mvb     | application/x-msmediaview               |
| nws     | message/rfc822                          |
| oda     | application/oda                         |
| p10     | application/pkcs10                      |
| p12     | application/x-pkcs12                    |
| p7b     | application/x-pkcs7-certificates        |
| p7c     | application/x-pkcs7-mime                |
| p7m     | application/x-pkcs7-mime                |
| p7r     | application/x-pkcs7-certreqresp         |
| p7s     | application/x-pkcs7-signature           |
| pbm     | image/x-portable-bitmap                 |
| pdf     | application/pdf                         |
| pfx     | application/x-pkcs12                    |
| pgm     | image/x-portable-graymap                |
| pko     | application/ynd.ms-pkipko               |
| pma     | application/x-perfmon                   |
| pmc     | application/x-perfmon                   |
| pml     | application/x-perfmon                   |
| pmr     | application/x-perfmon                   |
| pmw     | application/x-perfmon                   |
| pnm     | image/x-portable-anymap                 |
| pot,    | application/vnd.ms-powerpoint           |
| ppm     | image/x-portable-pixmap                 |
| pps     | application/vnd.ms-powerpoint           |
| ppt     | application/vnd.ms-powerpoint           |
| prf     | application/pics-rules                  |
| ps      | application/postscript                  |
| pub     | application/x-mspublisher               |
| qt      | video/quicktime                         |
| ra      | audio/x-pn-realaudio                    |
| ram     | audio/x-pn-realaudio                    |
| ras     | image/x-cmu-raster                      |
| rgb     | image/x-rgb                             |
| rmi     | audio/mid                               |
| roff    | application/x-troff                     |
| rtf     | application/rtf                         |
| rtx     | text/richtext                           |
| scd     | application/x-msschedule                |
| sct     | text/scriptlet                          |
| setpay  | application/set-payment-initiation      |
| setreg  | application/set-registration-initiation |
| sh      | application/x-sh                        |
| shar    | application/x-shar                      |
| sit     | application/x-stuffit                   |
| snd     | audio/basic                             |
| spc     | application/x-pkcs7-certificates        |
| spl     | application/futuresplash                |
| src     | application/x-wais-source               |
| sst     | application/vnd.ms-pkicertstore         |
| stl     | application/vnd.ms-pkistl               |
| stm     | text/html                               |
| svg     | image/svg+xml                           |
| sv4cpio | application/x-sv4cpio                   |
| sv4crc  | application/x-sv4crc                    |
| swf     | application/x-shockwave-flash           |
| t       | application/x-troff                     |
| tar     | application/x-tar                       |
| tcl     | application/x-tcl                       |
| tex     | application/x-tex                       |
| texi    | application/x-texinfo                   |
| texinfo | application/x-texinfo                   |
| tgz     | application/x-compressed                |
| tif     | image/tiff                              |
| tiff    | image/tiff                              |
| tr      | application/x-troff                     |
| trm     | application/x-msterminal                |
| tsv     | text/tab-separated-values               |
| txt     | text/plain                              |
| uls     | text/iuls                               |
| ustar   | application/x-ustar                     |
| vcf     | text/x-vcard                            |
| vrml    | x-world/x-vrml                          |
| wav     | audio/x-wav                             |
| wcm     | application/vnd.ms-works                |
| wdb     | application/vnd.ms-works                |
| wks     | application/vnd.ms-works                |
| wmf     | application/x-msmetafile                |
| wps     | application/vnd.ms-works                |
| wri     | application/x-mswrite                   |
| wrl     | x-world/x-vrml                          |
| wrz     | x-world/x-vrml                          |
| xaf     | x-world/x-vrml                          |
| xbm     | image/x-xbitmap                         |
| xla     | application/vnd.ms-excel                |
| xlc     | application/vnd.ms-excel                |
| xlm     | application/vnd.ms-excel                |
| xls     | application/vnd.ms-excel                |
| xlt     | application/vnd.ms-excel                |
| xlw     | application/vnd.ms-excel                |
| xof     | x-world/x-vrml                          |
| xpm     | image/x-xpixmap                         |
| xwd     | image/x-xwindowdump                     |
| z       | application/x-compress                  |
| zip     | application/zip                         |