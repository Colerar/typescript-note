`TypeScripts` 中主要的類型:

`number`, `string`, `boolean`, `literal`

`any`, `unknown`, `void`, `never`, `object`

`array`, `tuple`, `enum`

## literal

字面量(literal) 類型聲明如下:

```typescript
let sex: "male" | "female"; // | 代表聯合類型
// sex = "unknown" // error 不能将类型 "unknown" 分配给类型 "male" | "female"
sex = "female";
console.log(sex);
```

## any

`any` 類型就相當於 `JavaScript` 中的變量, 可以被賦值爲任何類型

強烈建議不要使用 `any` 更不要使用隱式的 `any`, 否則 `TypeScript` 將變爲和 `JavaScript` 等價的 `AnyScript`

```
let anyValue: any;
anyValue = 1;
anyValue = "10";
anyValue = false;
anyValue = [1, 2, 3, 4];
```

## unknown

`unknown` 是類型安全的 `any`, 若要將它賦值給其他變量, 必須經過類型檢查.

```typescript
let unknownValue: unknown;
unknownValue = 1;
// const string1: string = unknownValue // error 不能将类型“unknown”分配给类型“string”
if (typeof unknownValue === "string") {
  // 類型防護
  const string2: string = unknownValue; // 👍
}
if (unknownValue === "stringLiteral") {
  // 判斷相等
  const string3: string = unknownValue;
}
const string4: string = unknownValue as string; // 類型斷言
const string5: string = <string>unknownValue; // 另一種寫法
```

## void

`void` 表示不會返回值, 這類似 `Java` 裏的 `void`, `Kotlin` 裏的 `Unit`

當然, 如果你喫飽了沒事幹, 可以返回 `undefined` 和 `null`

```typescript
function voidFunc(number: number): void {
  if (number === 0) return undefined;
  if (number === 1) return null;
}
const voidResult = voidFunc(1); // void
const voidResult2 = voidFunc(0); // void
```

## never

`never` 表示一個永遠不存在的值, 常用的場景是拋出異常, 這類似 `Kotlin` 裏的 `Nothing`

```typescript
function throwError(): never {
  throw new Error("Error!");
}
function whileTrue(): never {
  while (true) {
    // 永遠不會停下
  }
}
```

## object

`object` 限制類型爲一個對象, 同時也可以限制, 該對象是一個**什麼樣的**對象.

應當把 `TypeScript` 中的對象類型看成對集合的描述, 類型和實例之間的屬性及其類型集合必須**相等**

同時這也是一種有動態類型特徵的鴨子類型, 對象不必言明一定繼承了某個接口, 只要功能相同, 就認爲他們是同一個類型.

```typescript
let objectJustHasName: { name: string }; // 該對象是一個有名字, 且沒有其他屬性的對象
objectJustHasName = { name: "🐱" };
// objectHasName = { name: "🐱", eat: "🐟"  } // 多了不行 error: 不能将类型“{ eat: string; }”分配给类型“{ name: string; }”。 对象文字可以只指定已知属性，并且“eat”不在类型“{ name: string; }”中。
let crowd: { count: number; highest: string; lowest: string };
// crowd = { count: 100 } // 少了也不行 error 类型“{ count: number; }”缺少类型“{ count: number; highest: string; lowest: string; }”中的以下属性: highest, lowest

let objectOptional: { name: string; eat?: string }; // 在名稱後面加 ?
objectOptional = { name: "🐱", eat: "🐟" }; // 有
objectOptional = { name: "🐱" }; // 沒有, 都可以

let objectHasName: { name: string; [property: string]: unknown }; // 只要求對象剛好滿足有 name 屬性的一種寫法
objectHasName = { name: "🐶", eat: "🦴" };
```

同樣地, 可以將這種寫法帶到函數上

```typescript
let combine: (a: number, b: number) => number;
combine = function (a, b) {
  return a + b;
}; // 此處是類型推斷, 不是 any
```

由此可以實現高階函數

```typescript
function count(
  list: Array<unknown>,
  method: (list: Array<unknown>) => number
): number {
  return method(list);
}
```

## array

數組的類型聲明:

```typescript
let array: number[] = [1, 2];
// or
let array1: Array<number> = [1, 2, 3];
```

## Tuple

`Tuple`:

```typescript
let h: [string, number];
// h = ['hello', 123, "a"] // error 不能将类型“[string, number, string]”分配给类型“[string, number]”。 源具有 3 个元素，但目标仅允许 2 个。
// h = ["hello"] // 不能将类型“[string]”分配给类型“[string, number]”。源具有 1 个元素，但目标需要 2 个。
h = ["123", 123]; // 👍
```

## Enum

```typescript
enum Gender {
  Male, // = 0
  Female, // = 1
}
```

## 類型聯合

```typescript
let j: { name: string } & { age: number }; // 同時實現兩個接口
```

## Typealias

```typescript
type Comparator = (a: number, b: number) => boolean;
const numComparator: Comparator = function (a, b) {
  return a > b;
};
```
