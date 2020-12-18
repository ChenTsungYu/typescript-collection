# 資料型別
## 原始型別 (Primitive Types)
`number`: 數值型別
```typescript
let isString: string = 'hello number type';
```
`string`: 字串型別
```typescript
let isString: string = 'hello string type';
```
`boolean`: 布林值
```typescript
let isBoolean: boolean = false;
```
`undefined`
`null`
`symbol`: 於 ES6 後引入的[新型別](https://www.fooish.com/javascript/ES6/Symbol.html)
`void`: JavaScript 沒有空值（Void）的概念，在 TypeScript 中， 代表某函數為不回傳值的狀態
```typescript
function symbolType(): void {
    alert('test symbolType!');
}
```
### 注意
1. `null` 跟 `undefined` 類型又被稱作 **Nullable Types**
2. 若想讓變數可以變成指定的型別外，也可以成為 **Nullable Type**，需要特別使用 `union` 將該變數註記為 `<YOUR-TYPE> | <nullable-type>` 的格式
範例:
```typescript
let ethierStringOrNull: string | null = null;

ethierStringOrNull = 'test';
ethierStringOrNull = null';
ethierStringOrNull = 'test';
```
上述方法可以同時成為 `string` 或者是 `null`

### `Null` 和 `Undefined` 兩者與 `void`  的差異
`undefined` 和 `null` 兩者為所有型別的子型別，換句話說，這兩類型別的變數，可以賦值給 `number` 型別的變數：
如: 
```typescript
// correct
let num: number = undefined;

// correct
let u: undefined;
let num: number = u;
```
`void` 型別的變數不能賦值給 `number` 型別的變數

```typescript
let u: void;
let num: number = u;

// Type 'void' is not assignable to type 'number'.
```
##  任意值型別 (Any)
表允許賦值為任意型別，相較於前面提到的普通型別，普通型別在賦值過程中是不被允許改變型別。
```typescript
let myFavoriteNumber: string = 'six';
myFavoriteNumber = 6;

//  error TS2322: Type 'number' is not assignable to type 'string'.
```
但若為 `any` 型別，**可允許被賦值為任意型別**
```typescript
let myFavoriteNumber: any = 'six';
myFavoriteNumber = 6;
```
### 注意
1. 宣告一個變數為任意值之後，對它的任何操作，返回的內容的**型別都是任意值**。
2. **Nullable Types** 被認為是 `any` 型別
### 任意值的屬性及方法
```typescript
let anyThing: any = 'test';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
```
### 未宣告型別的變數
若宣告變數時，未指定其型別，那麼它會被識別為**任意值(any)型別**
```typescript
let something;
// 等同  let something: any;
something = 'six';
something = 6;

something.setName('Tom');
```

### Delayed Initialization
對變數先做宣告，而不立即給值，該變數會無條件被視為 `any` 型別，所以不管後續代入什麼值進去，TS 都不會理會

## 物件型別 (Object Types)
`function`
`array`
`class`
`object` ->  用`Interfaces` 來定義


# 型別註記 (Type Annotation)與型別推論 (Type Inference)
## 型別推論
若變數沒有明確指定型別，那麼 TypeScript 會依照型別推論（Type Inference）的規則推斷出一個型別
如:
```typescript
let myFavoriteNumber = 'six';
myFavoriteNumber = 6;

// error TS2322: Type 'number' is not assignable to type 'string'.
```
等同下方
```typescript
let myFavoriteNumber: string= 'six';
myFavoriteNumber = 6;

// error TS2322: Type 'number' is not assignable to type 'string'.
```
上方兩個範例可看出 TypeScript 依據變數 `myFavoriteNumber` 的值來推定出型別為 `string`
### 注意
若宣告變數時沒有賦值，不管之後有無賦值，都會被推斷成 `any` 型別而**完全不被型別檢查**!
## 型別註記
開發者在宣告變數時即定義好型別。
### 目的
1. 開發者能明確知道變數固定在哪個型別
2. TS 能不靠推論知道變數的資料型別

## 簡單比較兩者差異
| Type Annotation | Type Inference |
| -------- | -------- | 
|  開發者宣告變數的資料型別給 TS 辨識 | TS 依據變數被賦予的值做推論    | 

## 聯合型別 (Union Types)
表示取值可以為多種型別中的一種，使用 `|` 來分隔每個型別
範例: 允許 myFavoriteNumber 的型別是 `string` 或者 `number`
```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'six';
myFavoriteNumber = 6;

// 可正常 work
```
若賦予非 `string` 或 `number`的值會出現下方錯誤
```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = true;

//  error TS2322: Type 'boolean' is not assignable to type 'string | number'.
//   Type 'boolean' is not assignable to type 'number'.
```
### 屬性及方法
聯合型別的變數在不確定是哪個型別時，只能存取此**聯合型別的所有型別裡共有的屬性或方法**
範例: 
```typescript
const getLength = (something: string | number) => {
    return something.length;
};

//  error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```
上述範例可知，`length` 並非 `string` 和 `number` 共有的屬性，會得到錯誤結果。
```typescript
const getString = (something: string | number) => {
    return something.toString();
};
```
變數若為聯合型別，被賦值時會依據**型別推論**的規則推測出一個型別
```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven'; // myFavoriteNumber 在此時被推斷成了 number 型別
console.log(myFavoriteNumber.length); // 5
myFavoriteNumber = 7; // myFavoriteNumber 在被推斷成 number 型別後被賦予 string  型別
console.log(myFavoriteNumber.length); // 由於 myFavoriteNumber已經是number 型別，編譯就會出錯
```




# 有價值的參考文章
- [型別推論 X 註記 - Type Inference & Annotation](https://ithelp.ithome.com.tw/articles/10214719)