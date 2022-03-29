# 針對遺留代碼加入單元測試的藝術 課後筆記


報名上了 91 的[單元測試課程](https://tdd.best/courses/unit-testing-gracefully-with-legacy-code-202203/)，把還有印象的東西寫下來，看能不能順利的在平常 coding 的時候用上 XD

<!--more-->

## 確認需求

- 你理解的跟我理解的是否一樣?
  - 可以故意舉有問題的情境，讓 PM 一起思考這個需求雙方理解是否一致
- API 對外參數越少越好，對外面來說，知道越少事就能達到相同的事會比較輕鬆

## isolated unit test
- 測試目標 (SUT, System Under Test / CUT, Class Under Test)
- 隔絕依賴，只測試單一功能
- Extract and override
  - 不適用 static class/method
  - 不適用不能繼承的類別
  - 找到外部依賴
  - 抽離成 protect virtual method (可 override)
  - 繼承 SUT，override method，fake 外部依賴
  - 用子類別執行測試
  - [很久以前的 Example](https://dotblogs.com.tw/hatelove/2015/11/26/unit-test-by-extract-and-override)

## 重構測試
1. SUT 抽離成 field，放 `[SetUp]` method
2. 依賴假物件，也要抽 field 放 `[SetUp]` method

    ```csharp
    private FakeHoliday _holiday;
    [SetUp]
    public void SetUp() {
        this._holiday = new FakeHoliday();
    }
    ```

3. 可精準描述該測試情境
4. 測試裡面應該只有情境
5. 就算更換 test framework，test code 應該不需要更動
6. GivenXXX, OOOShouldBeXXXX

    ```csharp
    [Test]
    public void today_is_xmas() {
        GiveToday(12, 25);
        SayHelloShouldReturn("Today is Christmas");
    }
    ```

## 測試方式
- Stub
  - 模擬外部相依物件的行為，讓測試可繼續下去
- Mock
  - 驗證外部相依物件的互動，先定義好結果，再執行 SUT 的動作
  - 只要跟定義不一樣，其餘行為都會報錯
- Spy
  - 驗證外部相依物件的互動，先執行 SUT 動作，再驗證是不是正確的行為
  - 只驗證你想驗證的互動

## 什麼時候要做測試
- ROI(投資報酬率)高的
  - 金流
  - Security
  - 商譽
  - 有發生過 bug 需要測試案例
- [Code Coverage 使用方式](https://tdd.best/blog/code-coverage-and-tdd/)

