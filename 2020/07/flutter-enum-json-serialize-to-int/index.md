# Flutter enum 做 Json 序列化時指定整數值

[`json_serializable`](https://pub.dev/packages/json_serializable) 預設會把 enum 序列化為字串，有時候需要序列化為整數值

<!--more-->

假設有個 class 定義如下

```dart
import 'package:json_annotation/json_annotation.dart';

part 'fruit.g.dart';

enum Fruit {
  apple,
  banana,
  orange,
}

@JsonSerializable()
class Test {
  final Fruit fruit;
  Test({this.fruit});
  factory Test.fromJson(Map<String, dynamic> json) => _$TestFromJson(json);
  Map<String, dynamic> toJson() => _$TestToJson(this);
}
```

序列化為 json 結果

```dart
print(jsonEncode(Test(fruit: Fruit.apple))); // {"fruit":"apple"}
```

如果想要讓 enum 表示為整數值，可以用 `@JsonValue` 指定列舉值

```dart
enum Fruit {
  @JsonValue(0)
  apple,
  @JsonValue(1)
  banana,
  @JsonValue(2)
  orange,
}
```

這樣結果會變成

```dart
print(jsonEncode(Test(fruit: Fruit.apple))); // {"fruit":0}
```

json_serializable 的 [example](https://github.com/google/json_serializable.dart/tree/master/example) 裡面似乎沒寫到 `JsonValue` 🤣

### Reference

- [json_serializable enum values in dart](https://stackoverflow.com/a/60820407/1568102)
- [json_annotation package](https://pub.dev/documentation/json_annotation/latest/)
- [json_serializable package](https://pub.dev/packages/json_serializable)

