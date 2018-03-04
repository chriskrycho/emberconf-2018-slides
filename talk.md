# TypeScript Up Your Ember.js App

## Session 1: TypeScript Intro

*****

## Introductions

* Chris Krycho
* Bill Pullen

### Just in case...

<https://github.com/chriskrycho/emberconf>

```
$ git clone https://github.com/chriskrycho/emberconf
$ yarn
```

Note: Hello, everyone, and welcome to the TypeScript Up Your Ember.js App workshop. I figured I’d start by introducing myself briefly and having my TA Bill introduce himself.

I’m a software engineer at Olo—we do white-label online ordering for restaurants. Our mobile web experience is an Ember.js application with about 20,000 lines of TypeScript. Right now, we are turning that mobile foundation into a responsive, progressive web application that will be _the_ white-label ordering experience at Olo. I’m also one of the maintainers of ember-cli-typescript, and an all-around nerd! We’ve been using TypeScript in our Ember app at Olo since late 2016—_well_ before it was easy or especially useful. But, happily, we’re now at a point where it’s both easy _and_ useful!

Bill?

So now I’d like to get a bit of a feel for where everyone is at in the room. We’re going to cover basically the same material no matter what, but I can pitch my discussion and adjust course and adjust speed depending on what people’s experience levels look like.

* How many of you have written any TypeScript before?
* How many of you have written any typed language _at all_ before? - C♯ or Java? - Elm, F♯, OCaml, PureScript, Haskell, etc.?

Cool! That’s really helpful, and we’ll make a point to make sure no one gets left behind.

I’ll say this again and again, but I really mean it: if you have a question, if something was confusing, really for any reason at all: stop me, and ask questions. I’ve intentionally left plenty of time for that and everyone in here will learn this better if you _do_ ask.

*****

## Schedule

* 9:00&ndash;9:50: **the basics of TypeScript**
    * 9:50&ndash;10:00: break

* 10:00&ndash;10:50: **converting Todo MVC from JavaScript to TypeScript**
    * 10:50&ndash;11:00: break

* 11:00&ndash;11:45: **refactoring Todo MVC with TypeScript** (working session)

* 11:45&ndash;12:00: **open discussion**

Note: Before we jump in, let’s talk through the basic approach I’m planning to take, just so everyone is on the same page.

* From now till about 9:45 or 9:50, I’m going to talk through **the basics of TypeScript**. This is going to be kind of “lecture”-style, but _please_ feel free to interrupt with questions. The point of this section is for us to go from _zero_ to a point where the rest of the workshop makes good sense.
* When we wrap that up, we’ll take a short break, till 10am. Breaks are really important because we all have to stretch and hit the bathroom, but they’re also really important in terms of _learning_. If we just try to crunch through, all of our brains will shut down. So we’ll let ourselves chill a bit, then dive back in.
* From 10:00 to about 10:50, we’ll work through **converting parts of the canonical “TODO MVC” Ember example app from JavaScript to TypeScript**. This will let us put into practice all the ideas we talk about in the first session. We’ll probably spend about half an hour of that working through a couple of those together, and then the remainder of that block you can work on the rest of the app at your own pace. We’ll be around to help you as you have questions or get stuck.
* After another break, from 11:00 to about 11:45, we’ll work on **refactoring with TypeScript**, still using the TODO MVC app as our baseline. In a lot of ways, this is where the best parts of using TypeScript will really show up.
* Finally, we’ll just spend the last 15 minutes on open discussion, questions, comments, etc. – anything Bill and I can answer from working on what is now (kind of hilariously) the largest and oldest Ember _TypeScript_ apps in the world, we’ll be happy to.

If any of you _have not_ cloned the repository and run `yarn` to get everything set up, this first session is a good time to do that in the background. The link on the whiteboard here will take you straight to it. (https://github.com/chriskrycho/emberconf)

*****

## TypeScript Basics

So let’s dive right in! Let’s talk about TypeScript!

---

### What _is_ TypeScript?

* _Basically_ a typed superset of JavaScript
* _Strictly_ a compile-to-JavaScript language
* But close enough that we can think of it that way

Note: TypeScript is _basically_ a typed superset of JavaScript. I say _basically_ because there are a few constructs in TypeScript which don’t exist in JavaScript. We’ll talk about those in a few minutes, but the fact that those exist means TypeScript is _strictly speaking_ a distinct language which compiles to JavaScript. For most purposes, though, it’s fine to think of it as a superset of JavaScript with types.

---

#### Cool, but why should I care?

Two big developer experience differences:

1.  Always-up-to-date documentation for functions and classes
2.  Many fewer "undefined is not an object"errors

And it’s not painful to use!

Note: So that’s all well and good, but _why should you care?_ Maybe that’s interesting if you’re (like me) kind of weirdly obsessed with type systems. But what does it gain you as JavaScript developer every day? How does it make your life easier?

1.  How many of you here like having docs for your functions? Now, how many of you would like it if those docs were always _right_ and _up to date_? Well, the first thing about TypeScript is that that’s exactly what it gives you. My experience of using TypeScript is _not_, for the most part, the way I’ve felt in some other programming languages, where I’m writing down names of things just because. It’s more like just documenting “for this function to work _at all_, it needs you to pass in a thing that has _this property_ on it”—and then finding out _in my editor_ if I passed in the wrong thing, or if my function doesn’t return what the docs say it does. So that’s handy.

2.  The second thing that makes TypeScript _really great_ is that it legitimately helps us ship fewer “undefined is not an object” kinds of errors to production. And I care about that not in the abstract sense but because every single one of those I ship to production means _something didn’t work for a user_. It also means it’s time I have to spend hunting down the cause of that bug instead of building something new—whether that new thing is adding a feature, or making the app work offline, or building a whole new product, or whatever else. TypeScript doesn’t get the count to zero, like some programming languages can—but it helps, a _lot_.

Finally, it’s worth note that it’s not _painful to use_ in the way some typed languages have been. If I need to write “This function needs an object with a `quack` method on it that I can call” I can just write that inline, and we’ll see that in a few minutes! The types _cost_ a lot less than they do in the sort of “typical” typed languages out there, which makes their relative value a lot higher, too.

---

### What _is_ TypeScript?

* _Basically_ a typed superset of JavaScript
* _Strictly_ a compile-to-JavaScript language
* But close enough that we can think of it that way

Note: Okay, so assuming that combo sounds like a win, _how_ does TypeScript do that, and especially in the context of an Ember.js app? To understand that, we need to spend the next few minutes getting a decent handle on TypeScript itself.

---

#### Structural typing

TypeScript has a _structural_ type system.

* Not like C++, Java, C♯
* More like OCaml/Reason, F♯, Elm

(Don’t panic! It’s okay if you don’t have experience with _any_ of those.)

Note: TypeScript is _not_ like the type systems most people have experience with from C++, Java, C♯, etc. It _is_ like the type systems of OCaml (including Reason), F♯, Elm, Haskell, etc. but most people’s idea of a type system comes from Java or C♯, and the differences can be very surprising.

---

<!-- .slide: data-transition="slide-in fade-out" -->

##### Structural typing: _Types are just shapes._

```ts
interface Person {
  firstName: string;
  lastName: string;
}
```

Note: In a structural type system, types are _just shapes_. Anything with that shape can be used wherever the shape is required. So in this example, we have a `Person` shape – we’ll talk more about using `interface` to define shapes later.

---

<!-- .slide: data-transition="fade" -->

##### Structural typing: _Types are just shapes._

```ts
interface Person {
  firstName: string;
  lastName: string;
}

function sayHello(p: Person) {
  console.log(Hello, ${p.firstName} ${p.lastName}!);
}
```

Note: Then we can define a function which takes a `Person`.

---

<!-- .slide: data-transition="fade" -->

##### Structural typing: _Types are just shapes._

```ts
interface Person {
  firstName: string;
  lastName: string;
}

function sayHello(p: Person) {
  console.log(Hello, ${p.firstName} ${p.lastName}!);
}

const person = {
  firstName: 'Chris',
  lastName: 'Krycho',
  favoriteHobby: 'running',
};
```

Note: Then we can create an object which _doesn’t_ explicitly call itself a `Person` but which _does_ match the shape.

---

<!-- .slide: data-transition="fade" -->

##### Structural typing: _Types are just shapes._

```ts
interface Person {
  firstName: string;
  lastName: string;
}

function sayHello(p: Person) {
  console.log(Hello, ${p.firstName} ${p.lastName}!);
}

const person = {
  firstName: 'Chris',
  lastName: 'Krycho',
  favoriteHobby: 'running',
};

sayHello(person);
```

Note: But we can use that with the `sayHello` function, because it has the right shape – even though it has _more_ properties than needed, and even though it doesn’t have the _name_ that we used to define that shape.

Again: shapes are the only thing TypeScript cares about! And this is a huge difference from C++ or Java or C♯, which all care whether the _name_ of the thing you’re using matches.

---

##### Structural typing: `class`

<ul>
<li class="fragment">*Names don’t matter.*</li>
<li class="fragment">A `class` is just a way to _define_ and _construct_ a shape.</li>
</ul>

```ts
class Person {
  firstName: string;
  lastName: string;

  constructor(first: string, last: string) {
    this.firstName = first;
    this.lastName = last;
  }
}

function sayHello(p: Person) {
  console.log(`Hello, ${p.firstName} ${p.lastName}!`);
}

const bill = { firstName: "Bill", lastName: "Pullen", employer: "Olo" };
sayHello(bill); // "Hello, Bill Pullen!
```
<!-- .element: class="fragment" -->

Note: Building on that: _names of types don’t matter_ in TypeScript. When you write a `class` definition, you’re just providing a definition of a shape, and a way to build that shape, all at once. So you can see here a function that takes a `Person`, and a `Person` is defined with a `class` – but TypeScript doesn’t care whether you constructed the shape using a class constructor an object literal. It only cares that you pass it something that matches the _shape_ you defined.

*****

### Type signatures

How do we actually write down these types?

Note: So let’s talk about how we actually write down types to use in TypeScript. That’ll give us the foundation we need for using them in the context of Ember.js specifically.

---

#### Basic types

* <!-- .element: class="fragment" --> Primitive types: `boolean`, `string`, `number`, `symbol`
* <!-- .element: class="fragment" --> Objects: `{ name: string }`
* <!-- .element: class="fragment" --> Arrays: `Array<number>` or `number[]`

Note: There are four “primitive” types in TypeScript: strings, booleans, numbers, and symbols. There are also _object_ and _array_ types.

* Object types look like object literals, but with _types_ instead of _values_ after the name of the field.
* Array types can be written two ways: as `Array<{the type, like "number" here}>` or `{the type, like "number" here}` followed by `[]`. We’ll come back to the version with `Array` written out explicitly in a few.

---

#### `any`

`any`: the great (and _terrible_) escape hatch.

```ts
function madWithPower(noLimits: any) {
    return noLimits.noHelpEither.ohNo;
}

madWithPower("just a string");  // 💥 at runtime
```

***
<!-- .element: class="fragment" data-fragment-index="1" -->

<blockquote class="fragment" data-fragment-index="1">
<p>"TypeError: undefined is not an object (evaluating 'noLimits.noHelpEither.ohNo')"</p>
</blockquote>

Note: TypeScript gives us an escape hatch, and I’ll tell you about it up front because it’s a useful tool while you’re converting your codebase, and for _rare_ occasions after that. But it’s also _dangerous_.

The `any` type is exactly what it sounds like. You’re telling TypeScript, “This can be anything, and don’t check me on _anything_ I do with it.” When you’re first converting an existing codebase, sometimes you _have_ to use this because it would be far too painful and too time-consuming to figure out every single type related to a given module as you go. We’ll see that in a few when we actually start converting some JavaScript over to TypeScript.

But it also means TypeScript _cannot help you_ with anything marked as being of the type `any`. No autocompletion. No type errors. Nothing. `any` is an escape hatch, but once you _do_ get your types written down, you should use it as a tool of last resort and be _very_ careful with runtime checks when you do bust it out.

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### Type ascriptions

Let’s write some basic types!

```ts
let myName: string = "Chris";
let theAnswer: number = 42;
let aLie: boolean = false;

function stringLength(s: string): number {
    return s.length;
}

let toString = (n: any): string => `${n}`;
```

Note: So when you’re using these types in your program, you’ll need to write down some types! Here are some types of things you might want to know how to write down:

* the primitive types
* functions

---

<!-- .slide: data-transition="fade" -->

##### Type inference

Most of those we didn’t actually need!

```ts
let myName = "Chris";
let theAnswer = 42;
let aLie = false;

function stringLength(s: string) {
    return s.length;
}

let toString = (n: any) => `${n}`;
```

Note: A lot of times, you _won’t_ have to write down types. Anywhere you assign a value, TypeScript _infers_ the type for you. Similarly, TypeScript will figure out function return types for you.

---

<!-- .slide: data-transition="fade-in slide-out" -->

##### Type inference

Most of those we didn’t actually need!

```ts
let myName = "Chris";  // automatically `string`
let theAnswer = 42;  // automatically `number`
let aLie = false;  // automatically `boolean`

function stringLength(s: string) { // automatically returns `number`
    return s.length;
}

let toString = (n: any) => `${n}`; // automatically returns string
```


---

<!-- .slide: data-transition="slide-in fade-out" -->

###### Type inference: quite sophisticated

```ts

function moreComplicated(a: boolean) {
  return a ? 'yay' : { say: 'what' };
}




console.log(moreComplicated(true).say);
```

Note: TypeScript can infer a _lot_. It’ll even infer more interesting types we haven’t talked about yet, like _union_ types. Here, TypeScript knows that we’re returning _either_ a string or an object with a key named “say” which has a string value. And when we go to use it, we’ll have to check what the type is, or TS will yell at us.

---

<!-- .slide: data-transition="fade-in slide-out" -->

###### Type inference: quite sophisticated

```ts
// returns `string | { say: string }`
function moreComplicated(a: boolean) {
  return a ? 'yay' : { say: 'what' };
}

// Type error! We haven't checked whether `say` exists, and
// in fact it *doesn't*, so this would blow up. But TS will
// save us.
console.log(moreComplicated(true).say);
```

---

<!-- .slide: data-transition="slide-in fade-out" -->

###### Type inference: limits

```ts
let aBunchOfThings = []; 

function whatIsThis(untypedThing) {
  
  
  return untypedThing.length;
}
```

Note: But it can’t infer _everything_. For example, if you create an empty array, you’ll need to tell it what _kind_ of array, or it’ll fall back to the `any` type.

You also have to write function _argument_ types explicitly pretty much all the time; TS doesn’t try to do total program inference like some other languages do.

Finally, when using _generic types_, which we’ll talk about in a minute, it will eventually fall over even when _you_ can see that there’s only a single type it could be. In that case, you _do_ have to write things out explicitly.

---

<!-- .slide: data-transition="fade" -->

###### Type inference: limits

```ts
let aBunchOfThings = []; // has type `any[]`

function whatIsThis(untypedThing) {
  
  
  return untypedThing.length;
}
```

---

<!-- .slide: data-transition="fade-in slide-out" -->

###### Type inference: limits

```ts
let aBunchOfThings = []; // has type `any[]`

function whatIsThis(untypedThing) {
  // Type error! We don't know that `untypedThing` *has*
  // a `length` property! It's actually `any` here.
  return untypedThing.length;
}
```

---

#### Writing shapes

Since TS is all about shapes, let’s write some!

* <!-- .element: class="fragment" --> `type` for “type aliases”
* <!-- .element: class="fragment" --> `interface` for “extensible shapes”
* <!-- .element: class="fragment" --> `class` for JS classes with annotations

Note: Since TypeScript is all about shapes, how do we write them?

---

<!-- .slide: data-transition="slide-in fade-out" -->

##### Writing shapes: `type`

```ts
function withGnarly(arg: {
  a: string[];
  b: number;
  c: { some: boolean };
}): boolean { /* ... */ }
```

Note: A type alias is a way of telling TypeScript “When I use this name, it’s just a shorthand for this shape!” We can write shapes inline, but that gets nasty quickly. So we can create an alias for them and use that instead.

---

<!-- .slide: data-transition="fade" -->

##### Writing shapes: `type`

```ts
type Arg = {
  a: string[];
  b: number;
  c: { some: boolean };
};

function withGnarly(arg: Arg): boolean { /* ... */ }
```


---

##### Writing shapes: `interface`

```ts
interface Name {
  primary: string;
}

interface WesternName extends Name {
  middle?: string;
  last: string;
}
```

Note: Another way to write a shape is with an `interface`, which can be _extended_ and _implemented_. These are basically interchangeable with type aliases, except for those two differences. Here, the `WesternName` has all the properties of `Name` and adds in an optional `middle` and required `last` name. You can use these wherever you’d use a type alias… but also with classes!

---

<!-- .slide: data-transition="slide-in face-out" -->

##### Writing shapes: `class`

TypeScript classes are just JavaScript classes with type annotations.

```ts
class Person {
  firstName: string;
  lastName: string;

  constructor(first: string, last: string) {
    this.firstName = first;
    this.lastName = last;
  }
}

function sayHello(p: Person) {
  console.log(`Hello, ${p.firstName} ${p.lastName}!`);
}
```

Note: A TypeScript class is _basically_ just a JavaScript class with type annotations for all the bits attached to it.

---

<!-- .slide: data-transition="fade-in slide-out" -->

##### Writing shapes: `class`

(But remember: they also define shapes!)

```ts
class Person {
  firstName: string;
  lastName: string;

  constructor(first: string, last: string) {
    this.firstName = first;
    this.lastName = last;
  }
}

function sayHello(p: Person) {
  console.log(`Hello, ${p.firstName} ${p.lastName}!`);
}

const bill = { firstName: "Bill", lastName: "Pullen", employer: "Olo" };
sayHello(bill); // "Hello, Bill Pullen!
```

Note: But it’s also worth remembering that a `class` declaration _also_ defines a shape in TypeScript. So you can also think of it as being a way to define a shape and a way to _build an instance_ of the shape at the same time.

---

##### Writing shapes: `extends` and `implements`

```ts
class Person implements Name {
  first: string;
  age: number;
}

class Programmer extends Person {
  knownJSFrameworks: string[];
}
```

Note: classes can also implement interfaces and extend other classes. If they declare they implement an interface, TypeScript will check that everything the interface has, the class has! Meanwhile `extends` works just like it does in normal JavaScript: you attach to the prototype of the type you’re extending.

---

##### Writing shapes: when to use each

<ul>
<li class="fragment" data-fragment-index="1">`type` as the default</li>
<li class="fragment" data-fragment-index="2">`interface` for defining shapes for more than one `class` to conform to</li>
<li class="fragment" data-fragment-index="3">`class` for a convenient way to get a shape and a constructor at the same time</li>
</ul>

Note: My basic tack is I start with a `type` alias, and rarely go beyond that. That’s where you really get the “this is just documentation my editor helps me check!” approach. I switch to an `interface` only if I’m going to define multiple `class`es that need to implement a certain shape contract. And I use `class` pretty rarely _other_ than when I’m building Ember components or services or whatever. (My own code is mostly just functions, and `type` aliases work _great_ with functions!)

I should note: I’m offering an opinionated take here. This actually runs up against Microsoft’s view a little bit – they basically say to use interfaces for everything and not to use type aliases at all, because things should _always_ be open to being extended. I disagree! I often want to just write down a bunch of small types like LEGO blocks to fit together. But there’s room for different styles here, in any case.

---

#### “nullable” types: getting a handle on `null` and `undefined`!

* the “optional” annotation
* strict null checking

Note: Next, one of the most important things TypeScript can help us with—I would argue, at least!—is `undefined` and `null`. How many people in this room have seen “undefined is not an object” or similar in the last week? Same. We’re slowly eradicating them from our app, but… it’s taking a while.

TypeScript gives us two tools we can combine to help us fix this problem: _optional_ type annotations, and the _strict null checking_ compiler option.

---

##### “nullable” types: the “optional” annotation

```ts
function mightConcat(a: string, b?: string) {
  return b ? a + ' ' + b : a;
}

type Name = {
  primary: string;
  surname?: string;
  other?: string[];
}
```

Note: The optional type annotation is just a question mark, applied to optional function arguments or optional properties on an object type.

Here, for example, we have a function which joins two strings... but only if the second string is supplied; otherwise it just returns the first string. The question mark tells TypeScript that it’s legitimate to leave off the second argument.

Likewise, if we were trying to build up a not-so-Western-focused version of a _name_ type, we might say that everyone has a primary name, but that a surname is optional, and that there are also optional other parts to the name. To build a name, we always need a `primary` value, but we can make a name with _neither_, _either_, or _both_ of the other keys.

---

<!-- .slide: data-transition="slide-in fade-out" -->

##### “nullable” types: strict null checking

Turn on `"strictNullChecking": true` in `tsconfig.json`!

```ts
let el: HTMLElement = document.querySelector('some-id');
el.focus();
```
<!-- .element: class="fragment" -->

Note: We can combine optional declarations with the `"strictNullChecking"` compiler option in our `tsconfig.json` file, which is where _all_ the compiler options go. We’ll look at that file a bit in the second session. For now, it’s just important to know that if we turn on `strictNullChecking`, anywhere that _could_ be `null` or `undefined`, TypeScript will require us to check for it! This can be a little annoying, but it means fewer bugs in production.

---

<!-- .slide: data-transition="fade-in slide-out" -->

##### “nullable” types: strict null checking

Turn on `"strictNullChecking": true` in `tsconfig.json`!

```ts
let el: HTMLElement = document.querySelector('some-id');
el.focus(); // Type error! This could be `null`!
```

Note: If you’re starting a _new_ Ember app with TypeScript, I’d turn this flag on at the start. If you’re dealing with an existing app... well, that’s probably going to be too hard, but it’s worth aiming to get there eventually!

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### Generics

```ts

let numbers = [1, 2, 3];


let strings = ['a', 'b', 'c'];


let things = [
  { thing: 1 },
  { thing: 2 },
];
```

Note: I’m not going to spend a _lot_ of time on generic types, although they’re both very _important_ and very _powerful_. We’ll see some examples of them in the refactoring section, and I’ll talk about them in more detail then. However, I think it’s worth introducing them so you recognize the syntax, and talking a _little_ about how to use them.

Generics let us capture things like the fact that we can have an array of just about anything: arrays of numbers, of strings, of objects, etc. If we want to be able write that down, especially for new types _we_ build, we need a syntax for it, to tell the compiler what we mean. That’s what generics are.

---

<!-- .slide: data-transition="fade" -->

#### Generics

```ts
// Array<number>
let numbers = [1, 2, 3];


let strings = ['a', 'b', 'c'];


let things = [
  { thing: 1 },
  { thing: 2 },
];
```

Note: You can see in the example here: an array can be an array of all sorts of things – numbers, strings, complex objects, etc. If we build up our _own_ containers that can hold more than one kind of thing, we can do that with generic types. I don’t expect to cover this much today, but you’ll _see_ it, so it’s helpful to know what the notation means!

---

<!-- .slide: data-transition="fade" -->

#### Generics

```ts
// Array<number>
let numbers = [1, 2, 3];

// Array<string>
let strings = ['a', 'b', 'c'];


let things = [
  { thing: 1 },
  { thing: 2 },
];
```

---

<!-- .slide: data-transition="fade-in slide-out" -->

#### Generics

```ts
// Array<number>
let numbers = [1, 2, 3];

// Array<string>
let strings = ['a', 'b', 'c'];

// Array<{thing: number }>
let things = [
  { thing: 1 },
  { thing: 2 },
];
```

*****

### Even snazzier kinds of types

* enums
* union types
* intersection types
* tuples
* literal types

Note: There are a handful more types you’ll see, and which can be _super_ useful. I’m not going to dig particularly deep into any of these, but I did want to touch on them before we start talking about Ember.js and TypeScript together.

---

#### `enum`

```ts
enum PrimaryColor {
  Red = 'FF0000',
  Green = '00FF00',
  Blue = '0000FF',
};

function fade(a: PrimaryColor, percent: number): string {
  // ...
}

convert(PrimaryColor.Red, 19);
```

Note: A TypeScript `enum` is _basically_ just a convenient way to define an object and a set of types associated with its values – to be able to say “The only thing allowed here is one of the values of this specific object.” So here, for example, we could define an RGB colors type and then we _have_ to pass in one of the `PrimaryColor` keys. We can’t pass in “purple” or even another hex color code string like `8800FF`!

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### literal types

```ts
type Hallo = {
  value: 'hallo';
};

let hallo: Hallo = {
  
  value: 'ahoy',
}
```

Note: _literal_ types are types where the only value they can have is the specific value you write down. Any kind of literal you can write in JavaScript – numbers, strings, booleans, symbols, arrays, objects, and crazy combinations of them – can be a _type_ in TypeScript. So here, the `value` key in any `Hallo` type is _only_ allowed to be the exact string “hallo”.

We’ll see a handy example of how we can use this in the _next_ kind of type: union types.

---

<!-- .slide: data-transition="fade-in slide-out" -->

#### literal types

```ts
type Hallo = {
  value: 'hallo';
};

let hallo: Hallo = {
  // Type error! This isn't the *exact* string we specified!
  value: 'ahoy',
}
```

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### union types

```ts
type Ok = { ok: true; value: number };
type Err = { ok: false; reason: string };
type Validation = Ok | Err;

function mightFail(succeed: boolean): Validation {
  return succeed
    ? { status: 'ok', value: 42 }
    : { status: 'err', reason: '...you said to fail!' };
}
```

Note: Union types are literally my favorite thing in TypeScript. They let you say “this thing can be _a_ or _b_.” And that’s a really common scenario! For example, we’ve all probably experienced a time when a given function needs to be able to indicate either success or failure – for example, a validation.

With union types, we can write that out, and TypeScript will check us: if we try to return `{ ok: false, value: 12 }` or `{ ok: true, reason: "whatever, man, you're not the boss of me" }`, it will complain. Here, it’s leaning on the literal types: the `Ok` type must include _exactly_ `ok: true` and `value: number` _or_ `ok: false` and `reason: string`.

---

<!-- .slide: data-transition="fade-in slide-out" -->

#### union types

```ts
type Ok = { ok: true; value: number };
type Err = { ok: false; reason: string };
type Validation = Ok | Err;

function mightFail(succeed: boolean): Validation {
  return succeed
    ? { status: 'ok', value: 42 }
    : { status: 'err', reason: '...you said to fail!' };
}

const yay = mightFail(true);
if (yay.ok) {
  console.log(yay.value); // can't touch `yay.reason` here
} else {
  console.log(yay.reason); // can't touch `yay.value` here.
}
```

Note: What’s also neat is that once you return one of these, TypeScript can figure out the type from the `ok: true` or `ok: false`. If you’re in a place where it’s `ok: true`, you can get at `value` but not `reason`, and vice versa.

---

#### intersection types

```ts
type HasName = { name: string };
interface HasMass { mass: number }
class LivingThing { age: number }

type Being = HasName & HasMass & LivingThing;
let me: Being = { name: "Chris", mass: 72, age: 30 };
```

Note: An _intersection_ type is the counterpart to a _union_ type. Instead of saying a value can be “this _or_ that” it says the value is “this _and_ that.” This is kind of like doing `extends` with an `interface`… except that you can just mix and match them however you want. (And notice that you can do intersections with any kind of TS shape!)

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### tuples

```ts
type NameAndAge = [string, number];
```

Note: TypeScript also lets use define _tuple_ types. These look a little like array types, but they’re different in an important way: they have a set length, and they have a set _order_. So if we defined a type for name and age, like this…

---

<!-- .slide: data-transition="fade" -->

#### tuples

```ts
type NameAndAge = [string, number];

// valid!
let good: NameAndAge = ["Chris Krycho", 30];
```

Note: then this would be valid…

---

<!-- .slide: data-transition="fade-in slide-out" -->

#### tuples

```ts
type NameAndAge = [string, number];

// valid!
let good: NameAndAge = ["Chris Krycho", 30];

// type errors!
let bad1: NameAndAge = [30, "Chris Krycho"];
let bad2: NameAndAge = ["Chris Krycho", 30, { is: 'a nerd' }]
```

Note: … but these would _not_! because the order is wrong in the first one, and the second has too many values.

These are handy for return types where you need to return more than one things – like in promise chains. If you have more complicated structures, though, you’re usually better returning objects, because names can add a lot of clarity.

---

## TypeScript Basics

<!-- .element: class="fragment" -->…actually, that's it!

<!-- .element: class="fragment" -->(I mean, okay: there *is* more, but that's enough to get started with!)

Note: Okay, so that’s it for TypeScript itself. We did not cover _everything_ in TypeScript, for sure, but we got through most stuff we’ll need. Any questions so far?

---

## TypeScript in Ember.js

<!-- .element: class="fragment" -->This is mostly just TypeScript!

<!-- .element: class="fragment" -->_(Mostly!)_

Note: Using TypeScript in Ember is _mostly_ just like using it in general, but there are some definite gotchas. These gotchas are for things _as they are_, but in the workshop session I’m going to introduce things _as they are about to be_, which alleviate these a lot!

*****

### `Ember.Object` (and everything related)

```ts
import EmberObject from '@ember/object';

const OldSchoolPerson = EmberObject.extend({
  firstName: undefined as string | undefined,
  lastName: undefined as string | undefined,
});

class NewSchoolPerson extends EmberObject {
  firstName?: string;
  lastName?: string;
}
```

Note: The first thing we have to deal with is `Ember.Object`. Mostly, things here work _fine_ using either the classic `.extend()` method or new-style classes. This goes direct uses of `Ember.Object`, like in this example, but it also goes for components, services, controllers, routes… _almost_ everything in the Ember world. Note that _almost_ everything I’m about to say here applies equally to classes in JavaScript.

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### `class` properties

```ts
import Component from '@ember/component';



export default class UserProfile extends Component {















}
```

Note: the syntax is a little different. Let’s walk through this _small_ example just to give you a taste. (We’ll cover this all again in the next session when we’re actually using TypeScript, so don’t worry if it doesn’t _stick_ just yet.)

---

<!-- .slide: data-transition="fade" -->

#### `class` properties

```ts
import Component from '@ember/component';



export default class UserProfile extends Component {
  name: string;
  age: number;













}
```

Note: We declare the types of the properties that get passed in, `firstName` and `lastName`.

---

<!-- .slide: data-transition="fade" -->

#### `class` properties

```ts
import Component from '@ember/component';



export default class UserProfile extends Component {
  name!: string;
  age!: number;













}
```

Note: As of TypeScript 2.7, they need an exclamation point here to tell TypeScript “this will _definitely_ be initialized” even though we don’t set it up in the constructor.

---

<!-- .slide: data-transition="fade" -->

#### `class` properties

```ts
import Component from '@ember/component';



export default class UserProfile extends Component {
  name!: string;
  age!: number;

  home = 'Colorado';











}
```

Note: Doing it _this_ way, we _assign_ properties with `=` instead of doing it key-value style with `:`

---

<!-- .slide: data-transition="fade" -->

#### `class` properties

```ts
import Component from '@ember/component';



export default class UserProfile extends Component {
  name!: string;
  age!: number;

  home = 'Colorado';
  hobbies: string[] = [];










}
```

Note: We can assign an array as a property directly; we don’t have to put it in `init` to avoid sharing between instances – because these _are_ instance properties.

---

<!-- .slide: data-transition="fade" -->

#### `class` properties

```ts
import Component from '@ember/component';
import { computed } from '@ember/object';


export default class UserProfile extends Component {
  name!: string;
  age!: number;

  home = 'Colorado';
  hobbies: string[] = [];

  desc = computed('name', 'age', function(this: UserProfile): string {
    return `${this.name} is ${this.age} years old!`;
  });






}
```

Note: In the `computed` property callback, we specify the `this` type. More on that in just a minute!

---

<!-- .slide: data-transition="fade" -->

#### `class` properties

```ts
import Component from '@ember/component';
import { computed } from '@ember/object';


export default class UserProfile extends Component {
  name!: string;
  age!: number;

  home = 'Colorado';
  hobbies: string[] = [];

  desc = computed('name', 'age', function(this: UserProfile): string {
    return `${this.name} is ${this.age} years old!`;
  });

  constructor() {
    super();


  }
}
```

Note: We use the normal JavaScript class `constructor` instead of the Ember-specific `init` method. You _can_ use `init` but don’t need to for normal setup stuff anymore.

---

<!-- .slide: data-transition="fade-in slide-out" -->

#### `class` properties

```ts
import Component from '@ember/component';
import { computed } from '@ember/object';
import { assert } from '@ember/debug';

export default class UserProfile extends Component {
  name!: string;
  age!: number;

  home = 'Colorado';
  hobbies: string[] = [];

  desc = computed('name', 'age', function(this: UserProfile): string {
    return `${this.name} is ${this.age} years old!`;
  });

  constructor() {
    super();
    assert('`name` is required', !!this.name);
    assert('`age` is required', !!this.age);
  }
}
```

Note: The constructor is a great place to check that you at least *received* the constructor arguments! Ember's `assert` will get stripped from production, so we do this alot.

You may also have noticed that I didn’t do `.get()` here. That’s because I’m doing _everything_ from this point in the workshop forward with Ember 3.1 in mind. For nearly everything, we just get to do `this.name` instead of `this.get('name')`. (Proxied values are the only exception!)

---

#### So which should I use?

Mostly, just use `class`-style declarations!

They’re better, _and_ easier to type-check!

Note: For most basic things, you can and should just use the class style of declaration everywhere! It’s nicer, and it’s _much_ easier to get type-checking, in part because of some of the hoops we have to jump through to get TypeScript playing nicely with all the neat-but-slightly crazy things Ember Object can do.

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### Prototype problems

Some things have to be on the prototype, not instance properties.

```ts
import Component from '@ember/component';

export default class UserList extends Component {
  tagName = 'ul';  // NOPE 👎
  items!: User[];
}
```

Note: There _are_ a few things `class` declarations cannot do that we rely on in Ember. The most important of these is setting or merging definitions on the prototype, rather than on a per-instance basis. That’s a pretty technical description; let’s make it concrete. When we create a `Component`, we can choose what its containing HTML tag should be with the `tagName` property. If we try to do this with a `class` property declaration, it simply won’t work. There are a couple workarounds for this.

---

<!-- .slide: data-transition="fade" -->

##### Prototype problems

Workaround: combine `.extend()` with `class`:

```ts
import Component from '@ember/component';

export default class UserList extends Component.extend({
  tagName: 'ul',   // works, but 🤔
}) {
  items!: User[];
}
```

Note: this is weird, but it works! We’ll see it again in a bit, too.

---

<!-- .slide: data-transition="fade-in slide-out" -->

##### Prototype problems

Workaround: use decorators!

```ts
import Component from '@ember/component';
import { tagName } from '@ember-decorators/component';

@tagName('ul')  // 👍 works!
export default class UserList extends Component {
  items!: User[];
}
```

Note: The other (better) option is decorators – which work for _most_ of these oddities. We’ll see a _bunch_ of these in way more detail in the second session as we walk through converting part of an app to TypeScript!

---

##### Using `class` and `.extend()`

```ts
class NewSchoolPerson extends EmberObject {
  firstName?: string;
  lastName?: string;
}

// NOPE 👎
const WithAMiddleName = NewSchoolPerson.extend({
  middleName: '',
});
```

Note: However, we should pretty much _always_ use classes with one exception: you cannot do `.extends()` and pass in a hash on a class you’ve defined with `class`. (This is all true whether you’re in TypeScript or not, but switching to TypeScript is the first place a lot of people run into it.) That means if you have a place where you _need_ that kind of prototype-merging we talked about a minute ago, _and_ you have a deep inheritance hierarchy, all the places back up the inheritance chain need to be defined as old-school Ember Objects, _not_ as classes.

This is a very practical reason to follow what is good advice anyway: avoid deep inheritance chains!

---

#### `this` types

```ts
import Component from '@ember/component';
import { computed } from '@ember/object';
import { assert } from '@ember/debug';

export default class UserProfile extends Component {






  desc = computed('name', 'age', function(this: UserProfile): string {
    return `${this.name} is ${this.age} years old!`;
  });






}
```

Note: As I commented above, we sometimes have to write down the `this` type for TypeScript to know what’s going on. You _normally_ won’t have to worry about this, because some of the changes for Ember 3.1 help, and so do decorators. The biggest times we have to use this are with old-style computed properties and old-style `actions` hashes. If we didn’t have `this: UserProfile` here, this wouldn’t be able to check that we’re setting a legitimate value, because the `this` context for actions (which Ember sets for us behind the scenes) wouldn’t be known to TypeScript. In both cases, decorators give us a nicer solution, but you _will_ see this if you’re converting existing code, so it’s worth knowing about.

*****

### “Type Registries”

Let's take a slight detour, into *type registries*.

***
<!-- .element: class="fragment" data-fragment-index="1" -->

<blockquote class="fragment" data-fragment-index="1"><p>These are some kind of arcane type magic!</p></blockquote>

***
<!-- .element: class="fragment" data-fragment-index="2" -->

<!-- .element: class="fragment" data-fragment-index="2" -->(Secret: they really aren't.)

Note: We’re going to take a brief detour to talk about some things you’ll see with service and controller injections and Ember Data lookups: *type registries*. This can look like crazy magic but it’s _not_.

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### What for?

How it looks *without* type registries:

```ts
import Component from '@ember/component';
import { inject as service } from '@ember/service';
import Computed from '@ember/object/computed';
import DS from 'ember-data';

import User from 'my-app/models/person';

export default class UserProfile extends Component {
  userId!: number;
  user!: User;

  store: Computed<DS.Store> = service();

  init(this: UserProfile) {
    const user = this.get('store').findRecord<User>('user', 123);
    this.set('user', user);
  }
}
```

Note: We want to be able to avoid massive boilerplate everywhere.

---

<!-- .slide: data-transition="fade-in slide-out" -->

#### What for?

How it looks *with* type registries:

```ts
import Component from '@ember/component';
import { inject as service } from '@ember/service';

import User from 'my-app/models/person';

export default class UserProfile extends Component {
  userId!: number;
  user: User;

  store = service('store');

  init(this: UserProfile) {
    const user = this.get('store').findRecord('user', 123);
    this.set('user', user);
  }
}
```

Note: Not only is there less boilerplate. This way, you also…

* get autocomplete for string arguments to `.findRecord` in your editor.
* get type errors if you type some _other_ method which doesn’t exist on the Ember Data store, or if you pass the wrong types to the `findRecord` method

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### A registry example

Registries actually aren't that complicated!

```ts
import Session from 'my-app/services/session';

declare module '@ember/service' {
  interface Registry {

  }
}
```

Note: With just this, we’ve integrated our service into the registry! Most apps will end up having a file with a _bunch_ of these imports which map names to a specific type. (I'm happy to talk about how the TS compiler makes all of this work together, but it's not that important for our purposes.)

---

<!-- .slide: data-transition="fade" -->

#### A registry example

Registries actually aren't that complicated!

```ts
import Session from 'my-app/services/session';

declare module '@ember/service' {
  interface Registry {
    session:
  }
}
```

They just map string keys (`session`)… <span class="invisible">to types (`Session`).</span>

---

<!-- .slide: data-transition="fade-in slide-out" -->

#### A registry example

Registries actually aren't that complicated!

```ts
import Session from 'my-app/services/session';

declare module '@ember/service' {
  interface Registry {
    session: Session;
  }
}
```

They just map string keys (`session`)… to types (`Session`).

*****

### Exceptions and Workarounds

<p class="invisible">*</p>

<p class="invisible">*</p>

<p class="invisible">*</p>

<p class="invisible">*</p>

Some things don’t work perfectly, of course. 🤕

---

<!-- .slide: data-transition="slide-in fade-out" -->

#### Ember Data

Back to the old *everything in `.extends()`* trick…

```ts
import DS from 'ember-data';
import CP from '@ember/object/computed';
import Metadata from 'my-app/lib/user/metadata';

export default class User extends DS.Model.extend({
  primaryName: DS.attr('string'),
  age: DS.attr('number'),
  metadata: DS.attr() as CP<Metadata>
}) {}
```

Note: The biggest problem here is Ember Data, where classes do not work _at all_ without decorators. To work around this, we use the “put it all in the `.extend()` hash” trick, while still giving ourselves a named type. We _also_ have to add type annotations where we don’t have a specific `DS.Transform` to point to. Here, we might know the type of a user’s “metadata” from elsewhere in the app; we just pull that type in to use with it.

As an aside: _anywhere_ we need to declare that a given item will be a computed property, this is how to do it. And in our app we actually usually use `CP` as the import name, but I wrote it out explicitly here to be clear.

---

<!-- .slide: data-transition="fade" -->

#### Ember Data

To actually use `class`, use decorators.

```ts
import DS from 'ember-data';
import { attr } from 'ember-decorators/data';
import CP from '@ember/object/computed';
import Metadata from 'my-app/lib/user/metadata';

export default class User extends DS.Model {
  @attr('string') primaryName: CP<string>;
  @attr('number') age: CP<number>;
  @attr metadata: CP<Metadata>
}
```

---

<!-- .slide: data-transition="fade-in slide-out" -->

#### Ember Data

Simplest (and recommended by @pzuraq/Chris Garrett):

```ts
import DS from 'ember-data';
import { attr } from 'ember-decorators/data';
import CP from '@ember/object/computed';
import Metadata from 'my-app/lib/user/metadata';

export default class User extends DS.Model {
  @attr primaryName: CP<string>;
  @attr age: CP<number>;
  @attr metadata: CP<Metadata>
}
```

<!-- .element: class="fragment" --> But none of these are great. 😢

<!-- .element: class="fragment" --> Hopefully it'll improve with time. 😬

---

#### Ember CLI Mirage

Also doesn’t play nicely with classes. 😭

Get around it the same way:

```
import Mirage, { faker } from 'ember-cli-mirage';

export default class User extends Mirage.Factory.extend({
  firstName: faker.name.firstName(),
  lastName: faker.name.lastName(),
}) {}
```

Note: Most of the same considerations apply with Mirage, and apparently for the same kinds underlying reasons. We use the same trick to get around it: add a class that `extends` from a normal `Mirage.Factory.extend()` invocation. That way we can use the actual class name and type and shape where we need it, but don’t have to

---

##### Ember CLI Mirage – Registries

<p class="invisible">*</p>

<p class="invisible">*</p>

<!-- .element: class="fragment" -->To make Ember CLI Mirage ergonomic in TypeScript, we need a type registry!

<!-- .element: class="fragment" -->Secret: I have built one in our app and will be upstreaming it soon!

<!-- .element: class="fragment" -->(Double secret: It’ll come with a quest issue to _type the whole ecosystem_.)

*****

### Limitations: templates and actions

<p class="invisible">*</p>

<p class="invisible">*</p>

<!-- .element: class="fragment" -->_Today_, TypeScript cannot help us with:

* <!-- .element: class="fragment" -->template bindings of any sort

* <!-- .element: class="fragment" -->including action invocation

<!-- .element: class="fragment" -->Maybe someday! 🤷🏽‍♂️

Note: _Today_, TypeScript cannot help us with template bindings of any sort, including action invocation. So we can write down the types of things passed into a component, and we can write down an action’s arguments in the component definition, but we have no guarantee that what we pass into a template matches that or that what we supply to an action is what it should be. I’ve had some early discussions with folks including core GlimmerJS developers about how we might work some magic there, but that’s a long way out.
