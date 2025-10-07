# Definition

JavaScript is a language made for the web development (client facing apps).

JS is a turn-based language (see [[JavaScript#Engine|JavaScript engines]] which execute js) or for simplicity single-threaded. 

This means that execution happens turn by turn (one task at a time).

```js
setTimeout(() => { //1
	console.log('delayed'); //3
}, 1000);

console.log('this goes first'); //2
```

Since JavaScript don't multitask withing the same thread -- it schedules tasks in a queue and processes them sequentially. 

That is why async features (such as setTimeout, [[JavaScript#Promise|Promises]], async/await) rely on the [[JavaScript#Event Loop|event loop]] to schedule future "turns".

# IIFE

Stands for *Immediately Invoked Function Expression*.

```js
(function () {
	console.log('This is my IIFE');
})
```

>[!INFO]
Literally means that anything within a function expression of this type will get executed ASAP.
>
>This is valuable in such cases where we don't require an specific functionality to be reused.

# Hoisting

This refers to JavaScript natural behavior of moving declarations to the top of their scope. 

Meaning that we can use variables and functions before even having them declared before-hand.

```js
sayHi();

function sayHi(){

}
```
# Closure


# Currying

This is a technique from functional programming where a function that takes multiple arguments is **transformed** (curried) into a *sequence of functions*, each taking a single argument.

```js
function sayHi(name, lastName, age){
	console.log(`Hi ${name} ${lastName} with age ${age}`);
}

function sayHiCurried(name){
	return function(lastName){
		return function(age){
			console.log(`Hi ${name} ${lastName} with age ${age}`);
		}
	}	
}
```

Here `sayHi` can also be written in its curried form `sayHiCurried`, and what changes essentially is the way it's called

```js
sayHi('john','doe',40);
sayHiCurried('john')('doe')(40);
```
## Use cases
### Partial application
We can create specialized functions by fixing some arguments:
```js
const multiply = a => b => a * b;

const double = multiply(2);
console.log(double(5)); // 10
console.log(double(8)); // 16
```

### Function composition
Currying allows us to leverage methods like map, filter or reduce. Like the following:
```js
const multiply = a => b => a * b;

const numbers = [1, 2, 3, 4];
const doubled = numbers.map(multiply(2));

console.log(doubled); // [2, 4, 6, 8]
```

In this way we can apply a transformation to an entire array.

# Context manipulation methods

## Call

## Apply

## Bind

# Promise

# Event Loop

# Engine
