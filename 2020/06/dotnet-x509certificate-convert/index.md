# 憑證 pem 檔轉換成 .NET 用的 pfx 檔 🔏


.NET 的 `X509Certificate2` 預設只能讀取 PFX/PKCS12 格式的檔案，手邊有專案產生的 `*.pem` 憑證，可以使用 openssl 轉換

<!--more-->

## openssl 語法

```shell
openssl pkcs12 -export -out <output> -in <certificate> -inkey <key> [-certifile <more certificate>]
```

## 範例

手邊有 cert.pem 跟 key.pem，需要轉檔成 cert.pfx

```shell
openssl pkcs12 -export -out cert.pfx -in cert.pem -inkey key.pem
```

下完命令後會需要輸入自訂憑證密碼

## Reference

- [Create a .pfx/.p12 Certificate File Using OpenSSL](https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/)
- [使用 OpenSSL 從 PFX 憑證文件匯出 PEM 憑證與金鑰](http://rocksaying.tw/archives/2019/使用OpenSSL從PFX憑證文件匯出PEM憑證與金鑰.html)

