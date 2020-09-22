# Unity IL2CPP 中傳 deletage 給 Native plugin


在 C# 傳 callback method 到 native plugin 裡面，會先包裝成 delegate，這次在執行的時候遇到 `NotSupportedException`

<!--more-->

## 症狀

Code 如下

- C++ side

```cpp
typedef void (* CallbackFunc)(int val);
extern "C"{
    void PowOf2(int val, CallbackFunc callback) {
        val *= val;
        callback(val);
    }
}
```

- C# side

```csharp
delegate void PowOf2Delegate(int val);
[DllImport("somelib")]
extern static void PowOf2(int val, PowOf2Delegate callback);

private void Start() {
    PowOf2(10, PowOf2Callback);    // << NotSupportedException
}

private void PowOf2Callback(int val) {
    Debug.Log(val);
}
```

Script backend 選 mono 的時候可正常執行，但選擇 IL2CPP 的時候，執行到 `PowOf2` 的時候會丟出 `NotSupportedException`

```txt
NotSupportedException: To marshal a managed method, please add an attribute named 'MonoPInvokeCallback' to the method definition.
```

## 解法
把 callback method 改成 static，並在前面加上 `MonoPInvokeCallback` 宣告

```csharp
[MonoPInvokeCallback(typeof(PowOf2Delegate))]
private static void PowOf2Callback(int val) {
    Debug.Log(val);
}
```

## Reference
- https://forum.unity.com/threads/making-calls-from-c-to-c-with-il2cpp-instead-of-mono_runtime_invoke.295697/
- https://forum.unity.com/threads/help-with-aot-compilation-with-monopinvokecallback.717917/
- https://shunzhitang.github.io/2019/03/iOS-iosRecord-Note/

