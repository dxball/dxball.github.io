# Cocos Component Circular Reference 警告


用之前 Unity 經驗在寫 cocos 元件的時候遇到一堆警告，搞了老半天才發現是怎麼回事

<!--more-->

當有繼承 Component 的類別互相引用對方，其類別屬性是對方類別且宣告成 `@property()`，可能就會發生以下錯誤

```text
 [Scene] Found possible circular reference in "pack:///mods/fs/0/assets/MyComponentB.js", happened when use "MyComponentA" imported from "./MyComponentA" Error
 [Scene] You are explicitly specifying `undefined` type to cc property "_a" of cc class "MyComponentB".
 [Scene] Please specifiy a default value for "MyComponentB._a" property at its declaration:
```

## 範例

```typescript
// MyComponentA.ts
import { Component, _decorator } from 'cc';
import { MyComponentB } from './MyComponentB';

const { ccclass, property } = _decorator;

@ccclass('MyComponentA')
export class MyComponentA extends Component {
    @property({ type: MyComponentB })
    private _b!: MyComponentB;
}
```

```typescript
// MyComponentB.ts
import { Component, _decorator } from 'cc';
import { MyComponentA } from './MyComponentA';

const { ccclass, property } = _decorator;

@ccclass('MyComponentB')
export class MyComponentB extends Component {
    @property({ type: MyComponentA })
    private _a!: MyComponentA;
}
```

## 解法

將其中一個類別另外寫一個 interface，`@property` 的型別改成 `Node`，再用 `Node.getComponent(className: string)` 的方式去取得對方元件即可

在 MyComponentB.ts 加入

```typescript
export interface IMyComponentB extends MyComponentB { }
```

MyComponentA.ts 修改為

```typescript
import { Component, Node, _decorator } from 'cc';
import { IMyComponentB } from './MyComponentB';

const { ccclass, property } = _decorator;

@ccclass('MyComponentA')
export class MyComponentA extends Component {
    @property({ type: Node })
    private _bNode!: Node;

    private _b: IMyComponentB | null = null;

    get b(): IMyComponentB {
        if (this._b == null) { this._b = this._bNode.getComponent('MyComponentB') as IMyComponentB }
        return this._b;
    }
}
```

## Reference
- https://discuss.cocos2d-x.org/t/circular-dependency-referencing-in-typescript/51377/6

