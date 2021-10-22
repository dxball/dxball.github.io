# 在 Win10 中計算檔案 hash 值


Linux 下可以用 `md5sum` 或 `sha1sum` 來計算 hash，現在 Win10 也有內建工具 `certutil` 可以用了。

<!--more-->

## 指令

```batch
certutil -hashfile <FileName> [HashAlgorithm]
```

其中 `HashAlgorithm` 支援 MD2 MD4 MD5 SHA1 SHA256 SHA384 SHA512

## 範例

![certutil](/2021/10/certutil_example.png)

## Reference
- https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certutil#-hashfile
- https://hackmd.io/@Not/rkh6ff5nd
- https://blog.darkthread.net/blog/compute-file-hash-in-win/

