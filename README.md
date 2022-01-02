# Linq for TypeScript

[![linqts](https://raw.githubusercontent.com/Lxsbw/linqts/master/linqts.png)](https://www.typescriptlang.org/)

## From

https://github.com/kutyel/linq.ts

Thank you

## Usage

```typescript
import { Linq } from 'https://deno.land/x/linqts/mod.ts';

let orderByID, persons, thenByAge, thenByName;

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

orderByID = new Linq<Person>(persons).OrderByDescending(x => x.ID).ToArray();

thenByAge = new Linq<Person>(persons)
  .OrderByDescending(x => x.ID)
  .ThenBy(x => x.Age)
  .ToArray();

thenByName = new Linq<Person>(persons)
  .OrderByDescending(x => x.ID)
  .ThenBy(x => x.Age)
  .ThenByDescending(x => x.Name)
  .ToArray();
```

## License

MIT
