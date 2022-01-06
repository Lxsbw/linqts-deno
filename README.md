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

### 2. Count

```typescript
const intArr = [1, 5, 8, 12, 15, 16];
const strArr = ['正一郎', '清次郎', '誠三郎', '征史郎'];

const rstInt = new Linq<number>(intArr).Count(x => x % 2 === 0);   // => 3
const rstStr = new Linq(strArr).Count();                           // => 4
```

### xx. ThenBy & ThenByDescending

```typescript
interface Person {
  ID: number;
  Age: number;
  Name: string;
}

persons = [
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
  .ThenByDescending(x => x.Name)
  .ToArray();

// =>
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
