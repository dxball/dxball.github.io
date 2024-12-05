# Windows Form 使用等寬字型


<!--more-->

因一些需求需要使用等寬字型(monospace)，除了直接指定字型名稱外，可以使用 `System.Drawing.FontFamily.GenericMonospace` 來用系統內建的等寬字型

```csharp
public partial class MyControl : UserControl {
    public MyControl() {
        InitializeComponent();
        this.Font = new System.Drawing.Font(System.Drawing.FontFamily.GenericMonospace, 8);
    }
}
```

參考: https://learn.microsoft.com/zh-tw/dotnet/api/system.drawing.fontfamily.genericmonospace

