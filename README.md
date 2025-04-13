# Linq for TypeScript

[![linqts](https://deno.land/x/linqts@1.5.0/linqts.png)](https://www.typescriptlang.org/)

## From

https://github.com/kutyel/linq.ts

## Usage

### import
```typescript
import { Linq } from 'https://deno.land/x/linqts/mod.ts';
```

### 1. all

```typescript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

const rst = new Linq<number>(numbers).all(x => x < 5);             // => false
```

### 2. any

```typescript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

const rst = new Linq<number>(numbers).any(x => x < 5);             // => true
```

### 3. average

```typescript
const parameters = [
  { Age: 0, Name: '正一郎' },
  { Age: 0.6, Name: '清次郎' },
  { Age: 0.09, Name: '誠三郎' },
  { Age: 0, Name: '征史郎' },
  { Age: 0, Name: '征史郎' },
  { Age: 0, Name: '征史郎' },
  { Age: 0, Name: '征史郎' },
  { Age: 0, Name: '征史郎' },
  { Age: 0, Name: '征史郎' },
  { Age: 0, Name: '征史郎' },
];

const rst = new Linq(parameters).average(x => x.Age); // => 0.069
```

### 4. count

```typescript
const strArr = ['正一郎', '清次郎', '誠三郎', '征史郎'];
const intArr = [1, 5, 8, 12, 15, 16];

const rstStr = new Linq(strArr).count();                           // => 4
const rstInt = new Linq<number>(intArr).count(x => x % 2 === 0);   // => 3
```

### 5. where & toArray

```typescript
const intArr = [0, 1, 2, 3, 4];
// even number
const rst = new Linq<number>(dataA).where(x => x % 2 === 0).toArray();  // => [ 0, 2, 4 ]
```

### 6. select & toArray

```typescript
const parameters = [
  { ID: 5, Rate: 0.0, Name: '正一郎' },
  { ID: 13, Rate: 0.1, Name: '清次郎' },
  { ID: 25, Rate: 0.0, Name: '誠三郎' },
  { ID: 42, Rate: 0.3, Name: '征史郎' }
];

const rst = new Linq(parameters)
  .select(x => { return { ID: x.ID, Name: x.Name }; }).toArray();
// =>
// [
//   { ID: 5, Name: "正一郎" },
//   { ID: 13, Name: "清次郎" },
//   { ID: 25, Name: "誠三郎" },
//   { ID: 42, Name: "征史郎" }
// ]
```

### 7. selectMany

```typescript
const parameters = [
  { Name: '正一郎', Numbers: [1, 2, 3] },
  { Name: '清次郎', Numbers: [1, 3, 5] },
  { Name: '誠三郎', Numbers: [2, 4, 6] },
  { Name: '征史郎', Numbers: [9, 8, 7] }
];

const rst = new Linq(parameters).selectMany(x => new Linq(x.Numbers)).toArray();  // => [1, 2, 3, 1, 3, 5, 2, 4, 6, 9, 8, 7]

```

### 8. distinct

```typescript
const intArr = [0, 1, 3, 3, 2];
const parameters = [
  { ID: 5, Rate: 0.0, Name: '正一郎' },
  { ID: 13, Rate: 0.1, Name: '清次郎' },
  { ID: 25, Rate: 0.0, Name: '正一郎' },
  { ID: 42, Rate: 0.3, Name: '征史郎' }
];

const rstInt = new Linq(intArr).distinct().toArray();              // => [ 0, 1, 3, 2 ]
const rstObj = new Linq(parameters).select(x => x.Name).distinct().toArray(); // => [ "正一郎", "清次郎", "征史郎" ]
```

### 9. distinctBy

```typescript
const data = [
  { id: 1, name: 'one', category: 'fruits', countries: ['lxsbw', 'xliecz'] },
  { id: 1, name: 'one', category: 'fruits', countries: ['Italy', 'Austria'] },
  { id: 2, name: 'two', category: 'vegetables', countries: ['Italy', 'Germany'] }
];

const rstKey = new Linq(data).distinctBy(x => x.category).toArray();
const rstKeys = new Linq(data)
  .distinctBy(el => {
    return { id: el.id, category: el.category };
  })
  .toArray();
// rstKey =>
// [
//   { id: 1, name: "one", category: "fruits", countries: [ "lxsbw", "xliecz" ] },
//   { id: 2, name: "two", category: "vegetables", countries: [ "Italy", "Germany" ] }
// ]
// rstKeys =>
// [
//   { id: 1, name: "one", category: "fruits", countries: [ "lxsbw", "xliecz" ] },
//   { id: 2, name: "two", category: "vegetables", countries: [ "Italy", "Germany" ] }
// ]
```

### 10. distinctMap

```javascript
interface DisType {
  ID: number;
  Rate: string;
  Name: string;
}

const parameters = [
  { ID: 5, Rate: 0.0, Name: '正一郎' },
  { ID: 13, Rate: 0.1, Name: '清次郎' },
  { ID: 25, Rate: 0.0, Name: '正一郎' },
  { ID: 42, Rate: 0.3, Name: '征史郎' },
  { ID: 19, Rate: 0.1, Name: '清次郎' },
  { ID: 45, Rate: 0.3, Name: '征史郎' },
  { ID: 26, Rate: 0.0, Name: '正一郎' },
  { ID: 27, Rate: 0.0, Name: '正二郎' }
];

let dataC_F = new Linq<DisType>(parameters).distinctMap(x => x.Name).toArray(); // => [ "正一郎", "清次郎", "征史郎" ]
let dataC_G = new Linq<DisType>(parameters)
  .distinctMap(x => {
    return { Name: x.Name };
  })
  .toArray(); // => [ { Name: '正一郎' }, { Name: '清次郎' }, { Name: '征史郎' } ]
```

### 11. first & firstOrDefault

```typescript
const numbers = [1, 2, 3, 5, 7, 11];
const parameters = [
  { ID: 5, Name: '正一郎' },
  { ID: 13, Name: '清次郎' },
  { ID: 25, Name: '誠三郎' },
  { ID: 42, Name: '征史郎' }
];

const rstInt = new Linq(numbers).first();                                      // => 1
const rstObj = new Linq<Person>(parameters).firstOrDefault(x => x.ID === 30);  // => undefined
const rstObj = new Linq<Person>(parameters).firstOrDefault(x => x.ID === 42);  // => { ID: 42, Name: '征史郎' }
```

### 12. remove

```typescript
let numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

console.log(numbers.length);                                    // => 10
new Linq(numbers).remove(6);
console.log(numbers.length);                                    // => 9
```

### 13. orderBy & orderByDescending

```typescript
interface Person {
  ID: number;
  Name: string;
}
const parameters = [
  { ID: 0, Name: '正一郎' },
  { ID: 3, Name: '清次郎' },
  { ID: 2, Name: '誠三郎' },
  { ID: 5, Name: '征史郎' }
];

const rst = new Linq<Person>(parameters).orderBy(x => x.ID).toArray();
const rstDesc = new Linq<Person>(parameters).orderByDescending(x => x.ID).toArray();
// rst =>
// [
//   { ID: 0, Name: "正一郎" },
//   { ID: 2, Name: "誠三郎" },
//   { ID: 3, Name: "清次郎" },
//   { ID: 5, Name: "征史郎" }
// ]
// rstDesc =>
// [
//   { ID: 5, Name: "征史郎" },
//   { ID: 3, Name: "清次郎" },
//   { ID: 2, Name: "誠三郎" },
//   { ID: 0, Name: "正一郎" }
// ]

```

### 14. thenBy & thenByDescending

```typescript
interface Person {
  ID: number;
  Age: number;
  Name: string;
}
const persons = [
  { ID: 0, Age: 30, Name: 'A' },
  { ID: 1, Age: 25, Name: 'B' },
  { ID: 2, Age: 2, Name: 'G' },
  { ID: 2, Age: 18, Name: 'C' },
  { ID: 1, Age: 30, Name: 'D' },
  { ID: 1, Age: 25, Name: 'E' },
  { ID: 2, Age: 15, Name: 'F' }
];

const rst = new Linq<Person>(persons)
  .orderByDescending(x => x.ID)
  .thenBy(x => x.Age)
  .thenByDescending(x => x.Name).toArray();
// 1 orderByDescending =>
// [
//   { ID: 2, Age: 2, Name: "G" },
//   { ID: 2, Age: 18, Name: "C" },
//   { ID: 2, Age: 15, Name: "F" },
//   { ID: 1, Age: 25, Name: "B" },
//   { ID: 1, Age: 30, Name: "D" },
//   { ID: 1, Age: 25, Name: "E" },
//   { ID: 0, Age: 30, Name: "A" }
// ]
// 2 thenBy =>
// [
//   { ID: 2, Age: 2, Name: "G" },
//   { ID: 2, Age: 15, Name: "F" },
//   { ID: 2, Age: 18, Name: "C" },
//   { ID: 1, Age: 25, Name: "B" },
//   { ID: 1, Age: 25, Name: "E" },
//   { ID: 1, Age: 30, Name: "D" },
//   { ID: 0, Age: 30, Name: "A" }
// ]
// 3 thenByDescending =>
// [
//   { ID: 2, Age: 2, Name: "G" },
//   { ID: 2, Age: 15, Name: "F" },
//   { ID: 2, Age: 18, Name: "C" },
//   { ID: 1, Age: 25, Name: "E" },
//   { ID: 1, Age: 25, Name: "B" },
//   { ID: 1, Age: 30, Name: "D" },
//   { ID: 0, Age: 30, Name: "A" }
// ]
```

### 15. orderBy local sort

```typescript
interface Person {
  Code: string;
  Name: string;
}
const parameters = [
  { Code: 'S', Name: '诗涵' },
  { Code: 'F', Name: '芳菲' },
  // { Code: 'U', Name: '悠悦' },
  { Code: 'H', Name: '慧琳' },
  { Code: 'E', Name: '恩琪' },
  { Code: 'R', Name: '睿萱' },
  { Code: 'B', Name: '碧琳' },
  { Code: 'C', Name: '采薇' },
  { Code: 'T', Name: '天翊' },
  { Code: 'G', Name: '冠宇' },
  { Code: 'Q', Name: '绮梦' },
  { Code: 'M', Name: '梦琪' },
  // { Code: 'V', Name: '薇雅' },
  { Code: 'Z', Name: '梓涵' },
  { Code: 'A', Name: '安雅' },
  // { Code: 'I', Name: '依诺' },
  { Code: 'Y', Name: '雅琴' },
  { Code: 'W', Name: '婉婷' },
  { Code: 'L', Name: '乐瑶' },
  { Code: 'K', Name: '可昕' },
  { Code: 'X', Name: '晓妍' },
  { Code: 'J', Name: '佳颖' },
  { Code: 'N', Name: '娜菲' },
  { Code: 'D', Name: '丹妮' },
  { Code: 'O', Name: '欧雅' },
  { Code: 'P', Name: '佩珊' },
];

const rst = new Linq<Person>(parameters, 'zh-CN').orderBy(x => x.Name).toArray();
// rst =>
// [
//   { Code: 'A', Name: '安雅' },
//   { Code: 'B', Name: '碧琳' },
//   { Code: 'C', Name: '采薇' },
//   { Code: 'D', Name: '丹妮' },
//   { Code: 'E', Name: '恩琪' },
//   { Code: 'F', Name: '芳菲' },
//   { Code: 'G', Name: '冠宇' },
//   { Code: 'H', Name: '慧琳' },
//   { Code: 'J', Name: '佳颖' },
//   { Code: 'K', Name: '可昕' },
//   { Code: 'L', Name: '乐瑶' },
//   { Code: 'M', Name: '梦琪' },
//   { Code: 'N', Name: '娜菲' },
//   { Code: 'O', Name: '欧雅' },
//   { Code: 'P', Name: '佩珊' },
//   { Code: 'Q', Name: '绮梦' },
//   { Code: 'R', Name: '睿萱' },
//   { Code: 'S', Name: '诗涵' },
//   { Code: 'T', Name: '天翊' },
//   { Code: 'W', Name: '婉婷' },
//   { Code: 'X', Name: '晓妍' },
//   { Code: 'Y', Name: '雅琴' },
//   { Code: 'Z', Name: '梓涵' }
// ]
```

### 16. groupBy

```typescript
interface GroupObj {
  id: number;
  name: string;
  category: string;
  countries: string[];
}
const data = [
  { id: 1, name: 'one', category: 'fruits', countries: ['lxsbw', 'xliecz'] },
  { id: 1, name: 'one', category: 'fruits', countries: ['Italy', 'Austria'] },
  { id: 2, name: 'two', category: 'vegetables', countries: ['Italy', 'Germany'] }
];

const rstKey = new Linq<GroupObj>(data).groupBy(el => el.category);
const rstKeys = new Linq<GroupObj>(data).groupBy(el => {
  return { id: el.id, category: el.category };
});
// rstKey =>
// [
//   {
//     key: 1, count: 2,
//     elements: [
//       { id: 1, name: "one", category: "fruits", countries: [Array] },
//       { id: 1, name: "one", category: "fruits", countries: [Array] }
//     ]
//   },
//   {
//     key: 2, count: 1,
//     elements: [ { id: 2, name: "two", category: "vegetables", countries: [Array] } ]
//   }
// ]
// rstKeys =>
// [
//   {
//     key: { id: 1, category: "fruits" }, count: 2,
//     elements: [
//       { id: 1, name: "one", category: "fruits", countries: [Array] },
//       { id: 1, name: "one", category: "fruits", countries: [Array] }
//     ]
//   },
//   {
//     key: { id: 2, category: "vegetables" }, count: 1,
//     elements: [ { id: 2, name: "two", category: "vegetables", countries: [Array] } ]
//   }
// ]
```

### 17. join

```typescript
const persons = [
  { CityID: 1, Name: 'ABC' },
  { CityID: 1, Name: 'EFG' },
  { CityID: 2, Name: 'HIJ' },
  { CityID: 3, Name: 'KLM' },
  { CityID: 3, Name: 'NOP' },
  { CityID: 4, Name: 'QRS' },
  { CityID: 5, Name: 'TUV' }
];
const cities = [
  { ID: 1, Name: 'Guangzhou' },
  { ID: 2, Name: 'Shenzhen' },
  { ID: 3, Name: 'Beijing' },
  { ID: 4, Name: 'Shanghai' }
];

const rst = new Linq(persons)
  .join(
    new Linq(cities),
    p => p.CityID,
    c => c.ID,
    (p, c) => {
      return { CityID: c.ID, PersonName: p.Name, CityName: c.Name };
    }
  )
  .toArray();
// rst =>
// [
//   { CityID: 1, PersonName: "ABC", CityName: "Guangzhou" },
//   { CityID: 1, PersonName: "EFG", CityName: "Guangzhou" },
//   { CityID: 2, PersonName: "HIJ", CityName: "Shenzhen" },
//   { CityID: 3, PersonName: "KLM", CityName: "Beijing" },
//   { CityID: 3, PersonName: "NOP", CityName: "Beijing" },
//   { CityID: 4, PersonName: "QRS", CityName: "Shanghai" }
// ]
```

### 18. toDictionary

```typescript
const parameters = [
  { ID: 0, Age: 52, Name: '正一郎' },
  { ID: 8, Age: 28, Name: '清次郎' },
  { ID: 3, Age: 20, Name: '誠三郎' },
  { ID: 4, Age: 18, Name: '征史郎' }
];

const dictionary = new Linq(parameters).toDictionary(x => x.ID).toArray();
const dictionaryObj = new Linq(parameters)
  .toDictionary(x => { return { ID: x.ID, Name: x.Name }; })
  .toArray();
// dictionary =>
// [
//   { Key: 0, Value: { ID: 0, Age: 52, Name: "正一郎" } },
//   { Key: 8, Value: { ID: 8, Age: 28, Name: "清次郎" } },
//   { Key: 3, Value: { ID: 3, Age: 20, Name: "誠三郎" } },
//   { Key: 4, Value: { ID: 4, Age: 18, Name: "征史郎" } }
// ]
// dictionaryObj =>
// [
//   { Key: { ID: 0, Name: "正一郎" }, Value: { ID: 0, Age: 52, Name: "正一郎" } },
//   { Key: { ID: 8, Name: "清次郎" }, Value: { ID: 8, Age: 28, Name: "清次郎" } },
//   { Key: { ID: 3, Name: "誠三郎" }, Value: { ID: 3, Age: 20, Name: "誠三郎" } },
//   { Key: { ID: 4, Name: "征史郎" }, Value: { ID: 4, Age: 18, Name: "征史郎" } }
// ]
```

### 19. sum

```typescript
interface Person {
  Age: number;
  Name: string;
}
const parameters = [
  { Age: 52, Name: '正一郎' },
  { Age: 28, Name: '清次郎' },
  { Age: 20, Name: '誠三郎' },
  { Age: 18, Name: '征史郎' }
];

const rst = new Linq<Person>(parameters).sum(x => x.Age);       // => 118
```

### 20. max

```typescript
interface Person {
  Age: number;
  Name: string;
}
const parameters = [
  { Age: 52, Name: '正一郎' },
  { Age: 28, Name: '清次郎' },
  { Age: 20, Name: '誠三郎' },
  { Age: 18, Name: '征史郎' }
];

const rst = new Linq<Person>(parameters).max(x => x.Age);       // => 52
```

### 21. min

```typescript
interface Person {
  Age: number;
  Name: string;
}
const parameters = [
  { Age: 52, Name: '正一郎' },
  { Age: 28, Name: '清次郎' },
  { Age: 20, Name: '誠三郎' },
  { Age: 18, Name: '征史郎' }
];

const rst = new Linq<Person>(parameters).min(x => x.Age);       // => 18
```

### 22. take

```typescript
const texts = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

const rst = new Linq(texts).take(4).toArray();                     // => [ "Sun", "Mon", "Tue", "Wed" ]
```

### 23. skip

```typescript
const texts = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

const rst = new Linq(texts).skip(4).toArray();                     // => [ "Thu", "Fri", "Sat" ]
```

## License

MIT
