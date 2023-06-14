## "this" in JS

`this` represents an object which executes the current function, it means that `this` is defined by the function execution context. _(This is partially true, we will know why. stay with this block)_
So, when a function is executed from a global context, "this" refers to the global object.

```
function globalExecution(){
    return this;
};
console.log(globalExecution()); // global object

let person = {
    firstName: "Arafat",
    lastName: "Ahmed",
    fullName: function () {
        return this.firstName + " " + this.lastName;
    }
};
console.log(person.fullName()); // Arafat Ahmed
```

In this scenario who is the executor of `person.firstName`?  `Person`, isn't it? Here, the `this` keyword will bind with the `Person` object.

Still the statement " `this` represents an object which executes the current function, it means that `this` is defined by the function execution context" is true.

But what about the arrow functions when arrow functions do have not their own execution context?

```
var myObject = {
    arrow_function: () => {
        console.log(this);
    }
};
myObject.arrow_function() // global object
```

What about now?
In this case, one could say, that `this` really depends on how the method is called, the same as normal functions,  but that's not the case here, let's see...
The arrow functions don't bind their own scope but inherit them from the parent one.

Let's change the example a little bit

```
var myObject = {
    arrow_function: null,
    myFunction: function () {
        this.arrow_function = () => { console.log(this) };
    }
};

myObject.myFunction() // this === myObject
myObject.arrow_function() // this === myObject

const myArrowFunction = myObject.arrow_function;
myArrowFunction() // this === myObject
```

When we call `myObject.myFunction()`, we initialize `myObject.arrow_function`  with an arrow function which is inside of the function `myFunction`, so it will inherit its scope.
