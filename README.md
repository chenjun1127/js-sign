## js生成接口请求参数签名加密

定义规则：将所有参数字段按首字母排序， 拼接成key1 = value1 & key2 = value2的格式，再在末尾拼接上key = appSecret， 再做MD5加密生成sign，方法如下：
```javascript
function getSign(params, kAppKey, kAppSecret) {
    if (typeof params == "string") {
        return paramsStrSort(params);
    } else if (typeof params == "object") {
        var arr = [];
        for (var i in params) {
            arr.push((i + "=" + params[i]));
        }
        return paramsStrSort(arr.join(("&")));
    }
}

function paramsStrSort(paramsStr) {
    var url = paramsStr + "&appKey=" + kAppKey;
    var urlStr = url.split("&").sort().join("&");
    var newUrl = urlStr + '&key=' + kAppSecret;
    return md5(newUrl);
}
```


调用：
```javascript
var params = "xid=" + xid + "&type=" + type + "&pageSize=" + pageSize + "&pageNo=" + pageNo;
var sign = getSign(params, kAppKey, kAppSecret);
```
或:
```javascript
var paramsObj = { xid: xid, pageSize: pageSize, type: type, pageNo: pageNo };
var sign = getSign(paramsObj, kAppKey, kAppSecret);
```

kAppKey，kAppSecret为常量，一般由后端定义好！签名加密方法依托[MD5](https://www.npmjs.com/package/blueimp-md5)!

