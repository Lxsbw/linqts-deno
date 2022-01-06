# Linq for TypeScript

[![linqts](https://deno.land/x/linqts@1.0.3/linqts.png)](https://www.typescriptlang.org/)

## From

https://github.com/kutyel/linq.ts

Thank you

## Usage

### import
```typescript
import { Linq } from 'https://deno.land/x/linqts/mod.ts';
```

### 1. Any

```typescript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

const rst = new Linq<number>(numbers).Any(x => x < 5);             // => true
```

### 2. All

```typescript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

const rst = new Linq<number>(numbers).All(x => x < 5);             // => false
```

### 3. Count

```typescript
const strArr = ['正一郎', '清次郎', '誠三郎', '征史郎'];
const intArr = [1, 5, 8, 12, 15, 16];

const rstStr = new Linq(strArr).Count();                           // => 4
const rstInt = new Linq<number>(intArr).Count(x => x % 2 === 0);   // => 3
```

### 4. Where & ToArray

```typescript
const intArr = [0, 1, 2, 3, 4];
// even number
const rst = new Linq<number>(dataA).Where(x => x % 2 === 0).ToArray();  // => [ 0, 2, 4 ]
```

### 5. Select & ToArray

```typescript
const parameters = [
  { ID: 5, Rate: 0.0, Name: '正一郎' },
  { ID: 13, Rate: 0.1, Name: '清次郎' },
  { ID: 25, Rate: 0.0, Name: '誠三郎' },
  { ID: 42, Rate: 0.3, Name: '征史郎' }
];

const rst = new Linq(parameters)
  .Select(x => { return { ID: x.ID, Name: x.Name }; }).ToArray();
// =>
// [
//   { ID: 5, Name: "正一郎" },
//   { ID: 13, Name: "清次郎" },
//   { ID: 25, Name: "誠三郎" },
//   { ID: 42, Name: "征史郎" }
// ]
```

### 6. SelectMany

```typescript
const parameters = [
  { Name: '正一郎', Numbers: [1, 2, 3] },
  { Name: '清次郎', Numbers: [1, 3, 5] },
  { Name: '誠三郎', Numbers: [2, 4, 6] },
  { Name: '征史郎', Numbers: [9, 8, 7] }
];

const rst = new Linq(parameters).SelectMany(x => new Linq(x.Numbers)).ToArray();  // => [1, 2, 3, 1, 3, 5, 2, 4, 6, 9, 8, 7]

```

### 7. Distinct

```typescript
const intArr = [0, 1, 3, 3, 2];
const parameters = [
  { ID: 5, Rate: 0.0, Name: '正一郎' },
  { ID: 13, Rate: 0.1, Name: '清次郎' },
  { ID: 25, Rate: 0.0, Name: '正一郎' },
  { ID: 42, Rate: 0.3, Name: '征史郎' }
];

const rstInt = new Linq(intArr).Distinct().ToArray();              // => [ 0, 1, 3, 2 ]
const rstObj = new Linq(parameters).Select(x => x.Name).Distinct().ToArray(); // => [ "正一郎", "清次郎", "征史郎" ]
```

### 8. First & FirstOrDefault

```typescript
const numbers = [1, 2, 3, 5, 7, 11];
const parameters = [
  { ID: 5, Name: '正一郎' },
  { ID: 13, Name: '清次郎' },
  { ID: 25, Name: '誠三郎' },
  { ID: 42, Name: '征史郎' }
];

const rstInt = new Linq(numbers).First();                                      // => 1
const rstObj = new Linq<Person>(parameters).FirstOrDefault(x => x.ID === 30);  // => undefined
const rstObj = new Linq<Person>(parameters).FirstOrDefault(x => x.ID === 42);  // => { ID: 42, Name: '征史郎' }
```

### 9. Remove

```typescript
let numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

console.log(numbers.length);                                    // => 10
new Linq(numbers).Remove(6);
console.log(numbers.length);                                    // => 9
```

### 10. Remove

```typescript
let numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

console.log(numbers.length);                                    // => 10
new Linq(numbers).Remove(6);
console.log(numbers.length);                                    // => 9
```

### 11. OrderBy & OrderByDescending

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

const rst = new Linq<Person>(parameters).OrderBy(x => x.ID).ToArray();
const rstDesc = new Linq<Person>(parameters).OrderByDescending(x => x.ID).ToArray();
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

### 12. ThenBy & ThenByDescending

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
  .OrderByDescending(x => x.ID)
  .ThenBy(x => x.Age)
  .ThenByDescending(x => x.Name).ToArray();
// 1 OrderByDescending =>
// [
//   { ID: 2, Age: 2, Name: "G" },
//   { ID: 2, Age: 18, Name: "C" },
//   { ID: 2, Age: 15, Name: "F" },
//   { ID: 1, Age: 25, Name: "B" },
//   { ID: 1, Age: 30, Name: "D" },
//   { ID: 1, Age: 25, Name: "E" },
//   { ID: 0, Age: 30, Name: "A" }
// ]
// 2 ThenBy =>
// [
//   { ID: 2, Age: 2, Name: "G" },
//   { ID: 2, Age: 15, Name: "F" },
//   { ID: 2, Age: 18, Name: "C" },
//   { ID: 1, Age: 25, Name: "B" },
//   { ID: 1, Age: 25, Name: "E" },
//   { ID: 1, Age: 30, Name: "D" },
//   { ID: 0, Age: 30, Name: "A" }
// ]
// 3 ThenByDescending =>
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

## License

MIT
