# 200504 工作碎唸


接手別人的專案實在 hen 痛苦 💔

<!--more-->

這幾天在處理公司另一個單位交接過來的程式，交接說明裡面寫說使用 MS SQL Server
但我在確認專案資料的時候卻發現有參考到 MySQL 的 Assembly 🙄

今天在修改一些問題的時候仔細研究了一下，終於發現是哪邊參考到了

```csharp
try {
    using(var db = new co_xxxx_entities()) {  // sql server connection
        // ....
    }
} catch (MySqlException ex) {
    // ....
}
```

不曉得為何連接到 SQL Server 的 code 會期待收到 `MySqlException` ... 🤦‍♂️

