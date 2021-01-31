# Windows Batch echo 數字到檔案


寫 batch 腳本時遇到的怪問題，將某些數字 echo redirect 到檔案時會消失不見

<!--more-->

## 狀況

有個需求是要用 batch 將指定的字串寫入檔案

```batch
echo set NUMBER=4>file.txt
```

最後得到的 file.txt 是空的

## 原因

數字搭配 `>` 或 `>>` Redirection 符號在 batch 中有特殊意義，詳見 [How-to: Redirection](https://ss64.com/nt/syntax-redirection.html)

> Numeric handles:
> - STDIN  = 0  Keyboard input
> - STDOUT = 1  Text output
> - STDERR = 2  Error text output
> - UNDEFINED = 3-9

## 解法

把 command 用 `()` 包起來後再 redirect 到檔案就可以了

```batch
(echo set NUMBER=4)>file.txt
```

另外還學到可以一次 echo 多行，腳本看起來比較漂亮

```batch
(
    echo set NUMBER=4
    echo set TYPE=test
    echo set BUILD_PATH=D:\output\
)>file.txt
```

或是

```batch
>file.txt(
    echo set NUMBER=4
    echo set TYPE=test
    echo set BUILD_PATH=D:\output\
)
```

## Reference
- https://stackoverflow.com/a/37731008/1568102
- https://ss64.com/nt/syntax-redirection.html

