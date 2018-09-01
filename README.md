/# Intro

Typescript is a superset of JavaScript. Installation
```
npm install -g typescript
```

# Configuration

Configuration is defined in:
```
tsconfig.json
``` 
Tip: Ctrl + Space will display available options for configuration.

```
{
    "compilerOptions": {
        "target": "es5", // there are multiple versions supported
        
    }
}
```

# Compilation

```
tsc .\app.ts
// or with wait for changes
tsc -w .\app.ts
// or faster
tsc -w
```

# Features

## Default parameters

```
function createUser(userName, isOptional = 1) {
```

## Template strings

```
var template = `${user.Name} expression ${user.isActive ? 0 : 1}`;
```

## Let and const

nothing special

## For of loops

```
for (let value of array) {
    console.log(value)
}
```

## Lambda

lambda function will be executed in a current context (opposite to regular function)
```
let lambda = () => x + 1;
```

## Destructuring

```
var [id, title, completed] = [first, second, third];
```

flip (swap) values
```
let a = 1;
let b = 2;
[a, b] = [b, a];
```

similar for objects
```
let { id, name } = { 1, "Name" };
```

object assigning
```
let { id, name} = user;
// or function
let { id, name } = getUser();
```
parameters
```
function countdown(...parameters)
//
function countdown({
    initial,
    final: final = 0,
    interval: interval = 1
}) {
}
```

## Spread Operator

support unlimited arguments
```
function add(action, ...values) {
    for (let value of values) {
        //...
    }
}
```

array values
```
var targetArray = [ 1, 2, ...sourceArray, 7, 8 ];
```

## Computed properties

Properties with dynamic names:
```
var support: {
    [prefix + 'property_1']: 1
    [prefix + 'property_2']: 2
}
```

# Type Fundamentals

ECMA5 Types: boolean, number, string, null, undefined, object, 
Special types: functions, array
Object literals: { }

## Parameter types

```
function length(x: string, y: string) : numbers {
    let total: number = x.length + y.length;
    return total;
}
```

If multiples types are acceptable:
```
function length(x: string | any[])
//or
function length(x: (string | any[]))
// then type could be checked by

if (x instanceOf String)
if (x instanceOf Array)
```

## Overload functions

Function with two overloads (will be compiled to one function in JavaScript, but helps to write TypeScript code):
```
function total(x: string)
function total(x: any[])
function total(x: string | any[]) {
}
```

# Custom Types

## Interfaces

Used only for compile time (no any JS generated for them).
* '?' means that property is optional

Interfaces could be:
* for object to define DTO object structure
* for service to define list of operations
```
interface Todo {
    name: string;
    completed?: boolean;
}

interface ITodoService {
    add(todo: Todo): Todo;
    getById(todoId: number): Todo;
    getAll(): Todo[];
    delete(todoId: number): void;
}
 
var todo = <Todo>{ ... };

var todo: Todo = {
    name: 'Name'
};
```

## Interfaces to define functions

```
interface jQuery {
    (selector: string): HTMLElement;
    varsion: number;
}

var $ = <jQuery>function(selector) {
}
```

## Extend interface

Any interface could be extended by creating interface with the same name:
```
interface baseInterface { ... }

interface baseInterface { // extended methods }
```

## Enums

```
enum TodoState {
    New = 1,
    Active = 2,
    Complete,
    Deleted
}

// in plan javascript it could be accessed as:
TodoState.Active // 2 
TodoState[2] // "Active"

// if you assign value, in plain Javascript it will be just a number:
var value = TodoState.Active;
// value will be 2
```

## Anonymous types

```
var todo: {
    name: string;
}
```

# Classes

## Prototypical inheritance

Way to find method:
* Check method on object
* Check prototype method
* Check base object prototype method and etc

```
function TodoService() {
    this.todos = [];
}

TodoService.prototype.getAll = function() {
    return this.todos;
}

var service = new TodoService();

service.getAll();
```

## Define class

```
class TodoService {

    // initialize variable
    todos: Todo[] = [];

    // initialize from constructor
    constructor(todos: Todo[]) {
        // Initialize in constructor
        this.todos = [];
    }

    getAll() {
        return this.todos;
    }
}
```

## Static properties

Instead of global variables, it is better to attach static to function:
```
function TodoService() {

}

// define
TodoService.lastId = 0;

// define in class style
class TodoService {
    static lastId: number = 0;
}

// access
TodoService.lastId
```

## Properties (getters, setters)

As usual, sometimes it much better to just use methods
```
var todo = {
    state: TodoState;

    get state() {
        return this.state;
    },
    set state(newState) {
        this.state = newState
    }
}

// usage
todo.state = TodoState.Complete;
```

## Inherit behaviour from a base class

Main constructions:
* extends
* super
```
class TodoStateChanger {

}

class CompleteTodoStateChanger extends TodoStateChanger {
    constructor() {
        super(parameters);
    }

    canChangeState(): boolean {
        return super.canChangeState();
    }
}
```

## Abstract class

```
abstract class TodoStateChanger {
    abstract canChangeState(todo: Todo): boolean;
}
```

## Control visibility

Access modifiers (no effect on runtime, only for development purpose):
* private - only the same class
* protected - private + inherited classes
* public
```
class TodoService {
    
    private static lastId: number;

    private getAll() {
        //
    }
}
```

## Implement interfaces

```
class TodoService implements ITodoService, IIdGenerator {

}
```

# Generics

```
// Original function
function clone(value) {
    let serialized = JSON.stringify(value);
    return JSON.parse(serialized);
}

// generic function
function clone<TModel>(value: TModel): TModel {
    let serialized = JSON.stringify(value);
    return JSON.parse(serialized);
}
```

Arrays are generics:
```
var array: Array<number> = [ 1, 2, 3]
```

Creating generic type:
```
class KeyValuePair<TKey, TValue> {

    constructor(key: TKey, value: TValue) {

    }
}

let pair = new KeyValuePair(1, 'First');
let pair = new KeyValuePair<number, string>(1, 'First');
```

## Constraints

Generic constraints - <T extends {length :number}>, could be:
- anonymous type
- interface
- any type
- types could be inherited
- but "extends" could not have generic types (in other words you could not set generic constraints)
```
function totalLength<TType extends { length: number }>(x: TType, y: TType) {
}
```

# Modules

Global variables and namespaces - very bad practice:
- uncontrolled sharing between components
- easily to break complex application
- difficult to determine components boundaries

Modules:
- Encourage more explicit dependencies
- Produce clearer component boundaries

## Namespaces

Define namespaces:
* using "namespace <name> { ... }"

Use namespaces:
* "import <name> = <namespace>;"

```
namespace TodoApp.Model: {
    enum Todo {
        New = 1
    }
}

namespace DataAccess {
    import Todo = TodoApp.Model.Todo;
    function todo() {
        let model: Todo;
    }
}
```

Namespaces:
* Are generated as immoderately invoked function expressions (IIFE)
```
(function defineType(TodoApp) {
    // code inside namespaces;
})(TodoApp || (TodoApp = {}))
``` 

## Internal and external modules

Internal modules (namespaces):
```
namespace TodoApp.Model {
    export interface Todo {
        // ...
    }
}
```

External modules:
* file scope
```
export interface Todo {
    // ...
}
```

## Imports

TypeScript supports both:
* require
* ECMAScript 2015 imports

To use export/import, proper configuration should be done in tsconfig:
"module":
* amd
* commonjs
* es2015
* es6
* system
* umd

## Loading external modules

There are multiple modules loaders:
* System.js

# Real Application

## Declaration files

* has extension ".d.ts" files
* defined types for existing JavaScript
* DefinitelyTyped (https://github.com/DefinitelyTyped/DefinitelyTyped) is just a simple repository on GitHub that hosts TypeScript declaration files for all your favorite packages
* could be generated with compiler option: "declaration" : true

Currently to install declaration files
```
npm install @types/<package>
```

## Source maps

Important for debugging support

# Decorators

Decorators are example of AOP, they could be defined for:
- methods
- properties (getters, setters)
- parameters

```
@log // log decorator
function add(input) {

}
```

implementation of new decorator:
```
function log(target: Object, methodName: string, descriptor: TypedPropertyDescriptor<Function>) {
}
```