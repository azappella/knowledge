# typescript

Typescript is a superset language of javascript which compiles to javascript.

## inferred types

For example, to create an object with an inferred type which includes name: string and id: number, you can write:

```js
const user = {
  name: "Hayes",
  id: 0,
};
```

## interfaces

You can explicitly describe this object's shape using an interface declaration:

```ts
interface User {
  name: string;
  id: number;
}
```

You can then declare that a JavaScript object conforms to the shape of your new interface by using syntax like : TypeName after a variable declaration:

```ts
const user: User = {
  name: "Hayes",
  id: 0,
};
```

## annotate params & return values

```ts
function deleteUser(user: User) {
  // ...
}

function getAdminUser(): User {
  //...
}
```

## js primitives

There is already a small set of primitive types available in JavaScript which you can use in an interface:

- boolean
- bigint
- null
- number
- string
- symbol
- undefined


TypeScript extends this list with a few more, such as `any` (allow anything), `unknown` (ensure someone using this type declares what the type is),
`never` (it’s not possible that this type could happen), and `void` (a function which returns undefined or has no return value).

## prefer interfaces

You’ll see that there are two syntaxes for building types: Interfaces and Types. You should prefer interface. Use type when you need specific features.

## composing types

### unions

With a union, you can declare that a type could be one of many types. For example, you can describe a boolean type as being either true or false:

```ts
type MyBool = true | false;
```

```ts
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

```ts
function getLength(obj: string | string[]) {
  return obj.length;
}
```

### generics

Generics provide variables to types. A common example is an array. An array without generics could contain anything.
An array with generics can describe the values that the array contains.

```ts
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

You can declare your own types that use generics:

```ts
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}

// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;

// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();

// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23);
Argument of type 'number' is not assignable to parameter of type 'string'.
```

## Structural Type System

One of TypeScript’s core principles is that type checking focuses on the shape that values have.
This is sometimes called “duck typing” or “structural typing”.

In a structural type system, if two objects have the same shape, they are considered to be of the same type.

```ts
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```

The point variable is never declared to be a Point type. However, TypeScript compares the shape of point to the shape of Point in the type-check.
They have the same shape, so the code passes.

The shape-matching only requires a subset of the object’s fields to match.

```ts
const point3 = { x: 12, y: 26, z: 89 };
logPoint(point3); // logs "12, 26"

const rect = { x: 33, y: 3, width: 30, height: 80 };
logPoint(rect); // logs "33, 3"

const color = { hex: "#187ABF" };
logPoint(color);
Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.
  Type '{ hex: string; }' is missing the following properties from type 'Point': x, y
```

There is no difference between how classes and objects conform to shapes:

```ts
class VirtualPoint {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const newVPoint = new VirtualPoint(13, 56);
logPoint(newVPoint); // logs "13, 56"
```

If the object or class has all the required properties, TypeScript will say they match, regardless of the implementation details.

Next steps: https://www.typescriptlang.org/docs/handbook/2/basic-types.html

source: https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html