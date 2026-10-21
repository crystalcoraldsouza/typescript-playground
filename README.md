# Wecome to Typescript refrsher

## 1. Basic types

### string — text

Use for names, letters, messages, IDs represented as text.

```js
let letter: string = "A";
```

### number — numeric values

Use for scores, points, tile counts and positions.

```js
let score: number = 5;
```

### boolean — true or false

Use for yes/no states, such as whether a move is valid.

```js
let isValid: boolean = true;
```

## 2. Arrays and tuples

### Type[] — a collection of values of the same type

Use when the number of items can vary, such as a player's tiles.

```js
const letters: string[] = ["A", "B", "C"];
const scores: number[] = [1, 3, 5];
```

### Tuple — a fixed sequence of specific types

Use when each position has a specific meaning, such as [row, column].

```js
const position: [number, number] = [3, 5];
```

## 3. Object types, interface and type

### Object type

Use to describe the shape of an object. Useful for a quick, local definition.

```js
const tile: { letter: string; points: number } = {
letter: "A",
points: 1
};
```

### interface — describe an object's structure

Use for reusable object structures, such as tiles, players or game boards.

```js
interface Tile {
letter: string;
points: number;
}

const tile2: Tile = { letter: "A", points: 1 };
```

### type — define a type or combine types

Use for object shapes too, but also for unions, tuples and other type combinations. type and interface overlap considerably.

```js
type Tile2 = {
letter: string;
points: number;
};
```

## 4. Union and literal types

### Union — |

Use when a value can legitimately be one of several types, such as a score that hasn't been calculated yet.

```js
let score2: number | null = null;
score = 10;
```

### Literal union

Use when only specific values are allowed. TypeScript rejects "paused" unless you include it in the union.

```js
type GameStatus = "waiting" | "playing" | "finished";

let status2: GameStatus = "waiting";
```

## 5. Functions and special types

### Function parameter and return types

Use parameter types to define what a function accepts. Return types are often inferred, but explicit ones can help clarify important contracts.

```js
function calculateScore(points: number): number {
return points \* 2;
}
```

### Optional properties — ?

Use when a property may be absent.

```js
interface Player {
name: string;
nickname?: string;
}
let player: Player = {
name: "susan",
nickname: "sue"
}
console.log(player.nickname?.toUpperCase()); // error is shown if no check of optional prop
```

### void

Use for functions whose return value isn't intended to be used.

```js
function announceTurn(): void {
console.log("Your turn!");
}
```

### unknown and any

Use unknown for values whose type you don't yet know; check the value before using it. any disables most type checking, so avoid it when possible.

```js
const getExternalInput = () => {}
let input: unknown = getExternalInput();
```

### Generics — `<T>`

Use when logic works with different types while preserving the specific type passed in.

```js
function firstItem<T>(items: T[]): T | undefined {
return items[0];
}
```

### enum

Use to represent a defined set of named constants. String literal unions are often a simpler alternative in modern TypeScript.

```js
enum Direction {
Across,
Down
}
```

### Promises

```js
async function getFavoriteNumber(): Promise<number> {
return 26;
}
```

### Inferred types for anonymous functions

```js
const names = ["Alice", 2, null];
```

#### Contextual typing for function - parameter s type is inferred

```js
names.forEach(function (s) {
  console.log(s);
});
```

#### Contextual typing also applies to arrow functions

```js
names.forEach((s) => {
  console.log(s);
});
```

### Union types

TypeScript will only allow an operation if it is valid for every member of the union. Narrowing occurs when TypeScript can deduce a more specific type for a value based on the structure of the code.

```js
function printId(id: number | string | string[] ) {
if (Array.isArray(id)) {
console.log(id.join(","))
} else if (typeof id === "string") {
console.log(id.toUpperCase()); // In this branch, id is of type 'string'
} else {
console.log(id); // Here, id is of type 'number'
}
}
```

#### Union type where all the members have something in common.

Return type is inferred as number[] | string

```js
function getFirstThree(x: number[] | string) {
return x.slice(0, 3);
}
```

### Type aliases

Its a name for any type that can be resused.

```js
type ID = {
x: number | string
}
```

> Note that aliases are only aliases - you cannot use type aliases to create different/distinct “versions” of the same type.

```js
type newString = string

let myNewString: newString = "Hello"

myNewString = "World" // string can be assigned to myNewString as itds still string under it all
```

### Interfaces

```js
interface ID2 {
x: number | string
}
```

### Differences Between Type Aliases and Interfaces

Type aliases and interfaces are very similar, and in many cases you can choose between them freely. Almost all features of an interface are available in type, the key distinction is that a type cannot be re-opened to add new properties vs an interface which is always extendable.

#### Interface

```js
interface Animal {
name: string;
}
interface Bear extends Animal {
honey: boolean;
}
interface Animal {
variant: string; // Adding new fields to an existing interface
}
const getBear = (): Bear => {
return { name: "Balloo", variant: "polar", honey: true}
}
const bear = getBear();
bear.name;
bear.honey;
```

```js
// Type
// A type cannot be changed after being created
type Animal2 = {
name: string;
}

type Bear2 = Animal2 & {
honey: boolean;
}
const getBear2 = (): Bear2 => {
return { name: "Balloo", honey: false}
}
const bear2 = getBear2();
bear.name;
bear.honey;
```

### Type Assertions

```js
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
// const myCanvas2 = <HTMLCanvasElement>document.getElementById("main_canvas"); // For tsx file
```

TypeScript only allows type assertions which convert to a more specific or less specific version of a type. This rule prevents “impossible” coercions like:

```js
// const z = "hellow" as number; // Not allowed
```

Sometimes this rule can be too conservative and will disallow more complex coercions that might be valid. If this happens, you can use two assertions, first to any (or unknown, which we’ll introduce later), then to the desired type:

```js
const z = "hellow" as any as number; // Now z has a type number
```

### Literal Types

```js
let updatingString = "test";
updatingString = "test2"; // The type is "string"

const updatingString2 = "Poppy"; // The type is teh string "Poppy" as its value cannot chnage
```

It’s not much use to have a variable that can only have one value!

```js
type Flowers = "Poppy" | "Sunflower" | "roses"

const updatingString3: Flowers = "Poppy"
```

### Literal Inference

```js
declare function handleRequest(url: string, method: "GET" | "POST"): void;

const req = { url: "https://example.com", method: "GET" };
// handleRequest(req.url, req.method); // Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.
```

You can change the inference by adding a type assertion in either location:

```js
// Change 1:
const req2 = { url: "https://example.com", method: "GET" as "GET" };
// Change 2:
handleRequest(req.url, req2.method as "GET");
```

You can use as const to convert the entire object to be type literals:

```js
const req3 = { url: "https://example.com", method: "GET" } as const;
handleRequest(req.url, req3.method); // The as const suffix acts like const but for the type system, ensuring that all properties are assigned the literal type instead of a more general version like string or number.
```

### null and undefined

if strictNullChecks is on, when a value is null or undefined, you will need to test for those values before using methods or properties on that value.

```js
function doSomething(x: string | null) {
if (x === null) {
// do nothing
} else {
console.log("Hello, " + x.toUpperCase());
}
}
```

### Non-null Assertion Operator (Postfix!)

Writing ! after any expression is effectively a type assertion that the value isn’t null or undefined

```js
function liveDangerously(x?: number | null) {
// No error
console.log(x!.toFixed());
}
```

### Narrowing

Narrowing occurs when TypeScript can deduce a more specific type for a value based on the structure of the code.

#### typeof type guards

TypeScript expects this to return a certain set of strings: "string", "number", "bigint", "boolean", "symbol", "undefined", "object", "function"

#### Truthiness narrowing

It’s fairly popular to leverage this behavior, especially for guarding against values like null or undefined

0, NaN, "" (the empty string), 0n (the bigint version of zero), null, undefined

!!"world"

#### Equality narrowing

The in operator narrowing

```js
type Fish = { swim: () => void };
type Bird = { fly: () => void };
type Human = { swim?: () => void; fly?: () => void };

function move(animal: Fish | Bird | Human) {
if ("swim" in animal) {
animal;
// ^?
} else {
animal;
// ^?
}
}
```

#### instanceof narrowing

```js
function logValue(x: Date | string) {
if (x instanceof Date) {
console.log(x.toUTCString());
// ^?
} else {
console.log(x.toUpperCase());
// ^?
}
}
```

### Control flow analysis

```js
function example() {
let x: string | number | boolean;
x = Math.random() < 0.5;
console.log(x); // let x: boolean
if (Math.random() < 0.5) {
x = "hello";
console.log(x); // let x: string
} else {
x = 100;
console.log(x); // let x: number
}
return x; // let x: string | number
}
```

### Using type predicates

```js
let pet: Fish | Bird; // pet could be either
// TypeScript knows pet could be either one, so it can't safely assume that pet.swim() exists.

function isFish(pet: Fish | Bird): pet is Fish {
return "swim" in pet;
}

// if (isFish(pet)) {
// pet.swim(); // TypeScript knows pet is Fish here
// } else {
// pet.fly(); // TypeScript knows pet is Bird here
// }
```

### Discriminated unions

```js
interface Shape {
kind: "circle" | "square";
radius?: number;
sideLength?: number;
}

function getArea(shape: Shape) {
if (shape.kind === "circle") {
return Math.PI \* shape.radius \*\* 2; // can use shape.radius!
}
return null
}
```

OR

```js
interface Circle {
kind: "circle";
radius: number;
}

interface Square {
kind: "square";
sideLength: number;
}

type Shape2 = Circle | Square;

interface Triangle {
kind: "triangle";
sideLength: number;
}

type Shape3 = Circle | Square | Triangle;

function getArea(shape: Shape3) {
switch (shape.kind) {
case "circle":
return Math.PI \* shape.radius ** 2;
case "square":
return shape.sideLength ** 2;
default:
const \_exhaustiveCheck: never = shape; // in case shape3 is added later this will give appropriate error
return \_exhaustiveCheck;
}
}
```

### Call Signatures

In JavaScript, functions can have properties in addition to being callable. However, the function type expression syntax doesn’t allow for declaring properties. If we want to describe something callable with properties, we can write a call signature in an object type.

```js
type Formatter = {
(value: string): string;
description: string;
};
const format: Formatter = (value) => value.toUpperCase();

format("hello"); // "HELLO"

format.description = "Converts text to uppercase";
```

### Construct Signatures

```js
type DateConstructorExample = {
new (value: string): Date;
};
const MyDate: DateConstructorExample = Date;

const today = new MyDate("2026-10-21");

console.log(today.getFullYear()); // 2026
// new in the type definition describes what the value must support. new in the actual code creates the object.
```

### Generic Functions

```js
function firstElement<Type>(arr: Type[]): Type | undefined {
return arr[0];
}
// s is of type 'string'
const s = firstElement(["a", "b", "c"]);
// n is of type 'number'
const n = firstElement([1, 2, 3]);
// u is of type undefined
const u = firstElement([]);
```

### Inference

```js
function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
return arr.map(func);
}

// Parameter 'n' is of type 'string'
// 'parsed' is of type 'number[]'
const parsed = map(["1", "2", "3"], (n) => parseInt(n));

// The keyof type operator
type Point = { x: number; y: number };
type P = keyof Point;

// Conditional types
interface Animal {
live(): void;
}
interface Dog extends Animal {
woof(): void;
}

type Example1 = Dog extends Animal ? number : string;
```
