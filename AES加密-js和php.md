>php使用ECB模式加密

```php
public function encrypt($data)
    {
    	//key必须是16位

        $data = openssl_encrypt($data, "AES-128-ECB", "1234567887654321", OPENSSL_RAW_DATA);
        return base64_encode($data);
    }

    public function decrypt($data)
    {
        return openssl_decrypt(base64_decode($data), "AES-128-ECB", "1234567887654321",  OPENSSL_RAW_DATA);
    }
```

>js使用cryptojs解密

```js

    //ECB模式(key必须是16位)
    AesKey = '1234567887654321';//加密时用的key,跟php一样
    message='bzdW6slzWa0zrm25cz9/6YMPKVMonH6Tc0RgZs0RmjQZa54zpNNvdPk0yOtdfgDCTa8I5d3trz8cmGkfyNlpcTsIH4zQsow3Cc+H0UXzW1nVD1LXVU84iiGB0YgXYPGPS4gSthMtbQtU4YG38MwZqiEn6+cEViAjAB/QWLuF669Dtjz5Lsll+85q8RmXvrBkE3I4grgl4m70cqv90iE/KHQcKsmH0BgUy4EWY1ue9qc=';//加密后的字符窜
    var ECBOptions = {
        mode: CryptoJS.mode.ECB,
        padding: CryptoJS.pad.Pkcs7
    };
    var key = CryptoJS.enc.Utf8.parse(AesKey);
    var bytes = CryptoJS.AES.decrypt(message, key,ECBOptions);
    var originalText = bytes.toString(CryptoJS.enc.Utf8);
    console.log(originalText)
```

>php使用CBC加密

>0 : 默认值，PKCS#7进行填充, 返回的数据经过 base64 编码;
>1 : OPENSSL_RAW_DATA, PKCS#7进行填充, 但返回的结果未经过 base64 编码;
>2 : OPENSSL_ZERO_PADDING, openssl不推荐chr(0)填充的方式, 需要开发者自行填充, 返回的结果经过 base64 编码;
>3 : OPENSSL_NO_PADDING, 需要开发者自行填充, 返回的结果不经过base64编码;
```php
public function cbc_encrypt($data)
    {
    	//key必须是16位
        return openssl_encrypt($data, "AES-128-CBC", "1234567887654321", 0,"1234567887654321");
    }

    public function cbc_decrypt($data)
    {
        return openssl_decrypt($data, "AES-128-CBC", "1234567887654321", 0,"1234567887654321");
    }
```

> js使用CBC解密

```js
encoded = 'n901\/9sHrIGeDRyNjy+xTG+e25J+GlDnixcKlrh7bGGmOpApbpgAdhnvWsIMzsUzU\/QFuKxPcU2eMUAWWn1FNKAfM7o9V7ELYJDIXpKRmRuWfVjddw\/sFesUKaqOrhZFTZLwe4HAdnOlLg4ThVM0O6IQGdwAg+FXuAYKOGZx375JU67mk9f63Ldx3d9f+GYIu7gcoDjiYEA0+9g4Y5ofp6zsjaPhuAxKsnrNtUACWVw='
    const key2 = CryptoJS.enc.Latin1.parse('1234567887654321');
    const iv2 = CryptoJS.enc.Latin1.parse('1234567887654321');
    const decoded = CryptoJS.AES.decrypt(encoded, key2, {
        iv: iv2,
        mode: CryptoJS.mode.CBC,
        //Zero模式解析后会出现补位\f的情况JSON解析会出错
        //{\"invite_time\":1660372882,\"team_id\":\"16583441365599\",\"type\":\"2\",\"user_id\":1,\"name\":\"123\",\"user_name\":\"\\u6c6a\\u4f1f\"}\f\f\f\f\f\f\f\f\f\f\f\f
        //padding: CryptoJS.pad.ZeroPadding
        padding: CryptoJS.pad.Pkcs7
    }).toString(CryptoJS.enc.Utf8)
    console.log('decoded', decoded);
```
>NoPadding：不进行补位操作，保持源数据。

>zeroPadding：末尾补0操作。对于源数据长度不是16的整数倍时，在末尾补0至长度为16的整数倍；

>另外一种情况是源数据长度正好是16的整数倍时，需要在数据末尾补16个0.（需注意！！！！！）

>PKCS5Padding ：对于源数据长度不是16的整数倍时，在末尾补(16-src_len%16)至长度为16的整数倍；

>另外一种情况是源数据长度正好是16的整数倍时，需要在数据末尾补16个16.（需注意！！！！！）

>PKCS7Padding:  与PKCS5Padding使用相同。

