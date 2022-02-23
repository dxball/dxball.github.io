# C#不使用 ";" 實作 Hello World


看到[別的語言](https://github.com/JacobLinCool/without-semicolon)的實作，也來試試看C#版的

<!--more-->

基本思路應該是要放在沒有 `;` 的語句裡面，像是 `if(){}` 或 `while(){}`，如果 method 有返回值的應該都可以簡單的呼叫就好，問題出在無返回值的 method

## 1. 使用 System.Reflection
第一個想到的就是用反射呼叫 `Invoke(object, object[])`，把 void method 轉成有回傳值的

```csharp
class Program {
    static int Main() {
        if(typeof(System.Console).GetMethod(nameof(System.Console.WriteLine),
            new System.Type[] { typeof(string) }).Invoke(null,
                new object[] { "Hello World" })) {}
    }
}
```

## 2. 使用 AsyncWaitHandle.WaitOne
轉成 `System.Action<string>` 後，用非同步的方式執行，再用 `WaitOne` 等待他執行完

```csharp
class Program {
    static int Main() {
        if(((System.Action<string>)System.Console.WriteLine)
            .BeginInvoke("Hello World", null, null).AsyncWaitHandle.WaitOne()) {}
    }
}
```

## 3. 使用 `is` 檢查
這個是[網路上查到](https://stackoverflow.com/a/32358864/1568102)的，我覺得最直覺的方法 XD，缺點是會跳 `CS0184` 警告，可以用 `#pragma warning disable CS0184` 隱藏起來

```csharp
#pragma warning disable CS0184
class Program {
    static int Main() {
        if(System.Console.WriteLine("Hello World") is object) { }
    }
}
```

