## Types

`TypeScript` 的變量聲明如下:

```typescript
// 使用 `let` 關鍵詞避免作用域異常
// 和 javascript 一樣, number 爲 IEEE 浮點數
let a: number; // 推薦
// 不推薦的做法, 不要使用 var 聲明變量
// var b: number; ❌
a = 1;
a = 114514;
a = 1.1451;
// a = 'a';
// 注意: typescript 默認配置是允許在 error 時仍然編譯成功的
```

`TypeScript` 具有一點類型推斷特性

```typescript
let c = true; // c: Any
c = false;
// c = "114514"; // error, c 被類型推斷爲 boolean
```

但是 `TypeScript` 的類型推斷十分有限, 對於變量, 僅能在聲明時同時賦值, 會自動推斷, 否則一律 Any

```typescript
let d; // 变量 "d" 隐式具有 "any" 类型，但可以从用法中推断出更好的类型。t
d = true;
```

於此同時, 不像 `Haskell`, `TypeScript` 不能自動推斷函數輸入. 因此, 以下代碼是純 `JavaScript` 的, 沒有用任何 `TypeScript` 特性:

```typescript
function sum(a, b) {
  // a: Any, b: Any
  return a + b;
}
console.log(sum(1, 4)); // 似乎運作正常
console.log(sum([1, 2, 3], [4, 5, 6])); // 你以爲會輸出 [1, 2, 3, 4, 5, 6] 嗎?
// 並不, 輸出的是 1, 2, 34, 5, 6
```

從上面的示例不難看出 `JavaScript` 有以下**優秀特性**:

- 🤮 噁心的隱式轉換
- 😢 難 DEBUG
- ❌ 運行時錯誤

毫無疑問, 這是使用 `TypeScript`, 拋棄 `JavaScript` 的最大原因.

如果加上類型限制...

```typescript
function sumNum(a: number, b: number): number {
  return a + b;
}
const result = sumNum(114, 514); // result: number = 628
// sumNum("114", 514) // error: 类型“string”的参数不能赋给类型“number”的参数。ts(2345)
```

<strong style="font-size: 2.0em"> 👍 Work Well </strong>
