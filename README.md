JavaScript Closures
===========

Closure sample. 

This is an attempt to clear up several (possible) misunderstandings about closures that appear in some of the other answers.

* A closure is not only created when you return an inner function. In fact, the enclosing function does not need to return at all in order for its closure to be created. You might instead assign your inner function to a variable in an outer scope, or pass it as an argument to another function where it could be called immediately or any time later. Therefore, the closure of the enclosing function is probably created as soon as the enclosing function is called since any inner function has access to that closure whenever the inner function is called, before or after the enclosing function returns.

* A closure does not reference a copy of the old values of variables in its scope. The variables themselves are part of the closure, and so the value seen when accessing one of those variables is the latest value at the time it is accessed. This is why inner functions created inside of loops can be tricky, since each one has access to the same outer variables rather than grabbing a copy of the variables at the time the function is created or called.

* The "variables" in a closure include any named functions declared within the function. They also include arguments of the function. A closure also has access to its containing closure's variables, all the way up to the global scope.

* Closures use memory, but they don't cause memory leaks since JavaScript by itself cleans up its own circular structures that are not referenced. IE memory leaks involving closures are created when it fails to disconnect DOM attribute values that reference closures, thus maintaining references to possibly circular structures.


//## Creating closure *without returning* a reference to the inner function

//### Just assign the inner function to a global variable or a variable in another scope.
//> *Everything* in the function scope is stored including the: 
//  * outer function arguments
//  * Variables
//  * Methods and Objects

```js
var x1;

function outer1(arg1) { //arg1 is also stored in closure
  //all variables are stored as references NOT copies of references
  var data = 'something';
  //all functions are also stored as references
  function say1() { return ' say1 ';}
  
  function inner1() {
    console.log( data + ' ' + arg1 + say1() );
  }
  
  x1 = inner1;
  
}

//call the outer function, this creates a closure
outer1(22);
//should print 'something 22 say1' because now a closure is created
x1();
```
//### Assign multiple variables to an inner function
//> What happens when we assign multiple variables to an inner function by calling the same outer function again?
//> Every invocation of outer function leads to a completely new closure so every reference is unique and never changed

```js

var x2, x3;

function outer2(arg2) {
  //all variables are stored as references NOT copies of references
  var data = 'something ' + arg2;
  //all functions are also stored as references
  function say2() { return ' say2 ';}
  
  function inner2() {
    console.log( data + ' ' + say2() );
  }
 
  if (arg2 === 22) {
    x2 = inner2;
  } else {
    x3 = inner2;
  }
}

outer2(22); //creates a new context/closure
x2();
outer2(23); //creates a new context/closure
x3();
x2(); //notice that calling x2 shows that the first context has not been destroyed
```

//### Create closure by returning an object ref instead of a function ref
```js
var x4;

function outer3( arg3 ) {
  //all variables are stored as references NOT copies of references
  var data = 'something ' + arg3;
  //all functions are also stored as references
  function say3() { return ' say3 ';}
  
  function inner3() {
    console.log( data + ' ' + say3() );
  }
  x4 = {"inner3": inner3, "data": data};
}

outer3('3');
x4.inner3();
console.log(x4.data);
```
