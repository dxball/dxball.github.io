# Notepad++ 使用 Regex capture group 取代特定格式字串


有時候我們需要把文件中某些特定字串改成另外的格式，以前我都是另外寫一隻程式去做字串處理，最近突然想到部分文字編輯器軟體有支援 Regular Expression 取代功能 (Notepad++, VSCode...)，這篇文章用來記錄 Notepad++ 的 regex 取代字串功能。

<!--more-->

假設有一份 Log 檔，內容如下

```txt
201013165206.637 |BOT|{Recv Command} 93;026;
201013165206.644 |BOT|{Send Command} 93;100;
201013165206.677 |DEBUG|[Manager] Save Image: /var/log/image/201013/xxx.img
201013165209.726 |BOT|Buffer=ABBCCDDDEEFFFGGG
201013165209.726 |BOT|{Recv Command} 94;021 ...
201013165209.741 |BOT|{Send Command} 94;100;
201013165209.742 |DEBUG|[Warning][10130010] Something error
201013165209.742 |DEBUG|[Manager] Clear Empty 4
201013165209.743 |DEBUG|[Manager] Clear complete
201013165209.779 |DEBUG|[Manager] Delete: /var/log/image/201012
201013165209.829 |DEBUG|[Manager] End Clear Old Data
```

其中每一列的最前面數字表示日期時間，格式為 `yyMMddHHmmss.fff`，需要把日期時間改為 `yy/MM/dd HH:mm:ss.fff`

## 操作
1. 用 Notepad++ 打開 Log
2. <kbd>CTRL</kbd>+<kbd>H</kbd> 打開取代視窗
3. 搜尋模式改為 `規則運算式`
4. 尋找內容輸入 `(\d{2})(\d{2})(\d{2})(\d{2})(\d{2})(\d{2}).(\d{3}) [|]`
5. 取代為輸入 `\1/\2/\3 \4:\5:\6.\7 |`
6. 按下全部取代  
![Replace window](/2020/10/notepad-plus-plus-regex-replace/notepad_plus_1.png)

## 結果
- 取代前  
![Before Replace](/2020/10/notepad-plus-plus-regex-replace/notepad_plus_2.png)
- 取代後  
![After Replace](/2020/10/notepad-plus-plus-regex-replace/notepad_plus_3.png)

## 說明
使用 Regular Expression 的 group 把搜尋字串拆解開來，再用參考變數取代
```
group    (\d{2})(\d{2})(\d{2})(\d{2})(\d{2})(\d{2}).(\d{3})
格式       yy     MM     dd     HH     mm     ss      fff
代表變數    \1     \2     \3     \4     \5     \6      \7
```
其中變數 `\n` 也可以寫成 `\{n}`、`$n`、`${n}`

## Reference
- [Notepad++ Capture Groups and Backreferences](https://npp-user-manual.org/docs/searching/#capture-groups-and-backreferences)
- [Boost format syntax](https://www.boost.org/doc/libs/1_55_0/libs/regex/doc/html/boost_regex/format/boost_format_syntax.html)
- [Notepad++ regex replace capture groups](https://blog.softhints.com/notepad-regex-replace-wildcard-capture-group/#notepadregexreplacecapturegroups)
- [Visual Studio Capture groups and replacement patterns](https://docs.microsoft.com/en-US/visualstudio/ide/using-regular-expressions-in-visual-studio#capture-groups-and-replacement-patterns)

