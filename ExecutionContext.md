
# Execution Context

A theoretical environment created by JS for code evaluation and execution.

Three types of execution in JS:

1. __Global Execution Context:__ Primary or base execution context, has 2 main functions:
    - Create a global object
    - Attach the `this` value to the global object
2. __Function Execution Context:__ The execution context created for every function invocation (created when a function is called).
3. __Eval Execution: ...__

--------------------------------------------------------------------------------

## Execution ( Call ) Stack

 STEP 1: Global execution context is pushed on the Call stack.
 STEP 2: Function execution context is created and pushed on the Call stack when a function is called.
 STEP 3: The invoked function is executed and removed from the stack, along with its execution context.

Example:

```JS
function person(){
 sayName();
}

function sayName(){
 console.log("Arafat Ahmed");
}

// invoke the ‘person ‘ function
person();
```

Step 1:  
|           |
|-----------|
|           |
|           |
|global()   |

Step 2:  
|           |
|-----------|
|           |
|person()   |
|global()   |

|           |
|-----------|
|sayName()  |
|person()   |
|global()   |

Step 3:
|           |
|-----------|
|           |
|person()   |
|global()   |

|           |
|-----------|
|           |
|           |
|global()   |

## Execution context

This execution is completed in 2 stages:

1. Creation stage,
2. Execution stage

Each step has 2 parts:

1. Lexical environment
    - Has 3 components:
        - Environment Record
        - Ref to an outer environment
        - `this` binding
    - Is 2 types:
        - __Declaration environment record:__\
            This type is used by a Lexical environment created in the Function execution context.
        - __Object environment record:__\
            A Lexical environment mainly uses this created in the Global execution context.
2. Variable environment
    - The structure is the same as the Lexical environment but this environment is used especially by the `var` variable statement.

>theoretically,
>
>```JS
>    executionContext = {
>        lexicalEnvironment = <ref>,
>        variableEnvironment = <ref>
>    }
>```

---------------------------------------------------------------------------------------------------

EXAMPLE:

```JS
let a = 1;
var b = 2;

function person(){
 sayName("Hello");
}

function sayName(greeting){
 console.log(greeting + " Arafat Ahmed");
}

// invoke the "person" function
person();
```

execution of this code will end in two stages

- Creation stage
- Execution stage

__Creation Stage__

```JS
globalExecutionContext:{
    "lexicalEnv":{
        environmentRecord:{
            type: "Object",
            a: <initialization>,
            person: <func>,
            sayName: <func>
        }
        outerEnv: <null>,
        thisBinding: <Global Object>
    },

    "variableEnv":{
        environmentRecord:{
            type: "Object",
            b: <initialization with "undefined">
        },
        outerEnv: <null>,
        thisBinding: <Global Object>
    }
}
```

When the `person` function is called, a new Function execution context is created to execute the function code.

```JS
functionExecutionContext:{
    lexicalEnv:{
        environmentRecord:{
            type: "Declarative",
            args: {length: 0},
        }
        outerEnv: <Global Env>,
        thisBinding: <Global Object or undefined>
    },

    variableEnv:{
        environmentRecord:{
            type: "Declarative",
        }
        outerEnv: <Global Env>,
        thisBinding: <Global Object or undefined>
    }
}
```

When the sayName function is called in person function another new Function execution context will created.

```JS
functionExecutionContext:{
    lexicalEnv:{
        environmentRecord:{
            type: "Declarative",
            args: {0: "Hello", length: 1},
        },
        outerEnv: <Global Env>,
        thisBinding: <Global Object or undefined>
    },

    variableEnv:{
        environmentRecord:{
            type: "Declarative",
        },
        outerEnv: <Person Env>,
        thisBinding: <Global Object or undefined>
    }
}
```

__Execution Stage__

```JS
globalExecutionContext:{
    lexicalEnv:{
        environmentRecord:{
            type: "Object",
            a: 1,
            person: <func>,
            sayName: <func>
        }
        outerEnv: <null>,
        thisBinding: <Global Object>
    },

    variableEnv:{
        environmentRecord:{
            type: "Object",
            b: 2
        },
        outerEnv: <null>,
        thisBinding: <Global Object>
    }
}
```

```JS
functionExecutionContext:{
    lexicalEnv:{
        environmentRecord:{
            type: "Declarative",
            args: {},
        }
        outerEnv: <Global Env>,
        thisBinding: <Global Object>
    },

    variableEnv:{
        environmentRecord:{
            type: "Declarative",
        }
        outerEnv: <Global Env>,
        thisBinding: <Global Object>
    }
}
```

When the sayName function is called in person function another new Function execution context will created.

```JS
functionExecutionContext:{
    lexicalEnv:{
        environmentRecord:{
            type: "Declarative",
            args: {0: "Hello", length: 1},
        },
        outerEnv: <Global Env>,
        thisBinding: <Global Object>
    },

    variableEnv:{
        environmentRecord:{
            type: "Declarative",
        },
        outerEnv: <Person Env>,
        thisBinding: <Global Object>
    }
}
```

------------------------------------------------------------------------------------------------------------------------------------

Now, we make a little change to our previous example

```JS
let a = 1;
var b = 2;

function person(greeting){
    return function(){
         console.log(greeting + " Arafat Ahmed");
    }
}

const sayName = person("Helo");
sayName();
```

What will happen?

__Creation stage__

```JS
globalExecutionContext:{
    "lexicalEnv":{
        environmentRecord:{
            type: "Object",
            a: <initialization>,
            person: <func>,
            sayName: <initialization>,
        }
        outerEnv: <null>,
        thisBinding: <Global Object>
    },

    "variableEnv":{
        environmentRecord:{
            type: "Object",
            b: <initialization with "undefined">
        },
        outerEnv: <null>,
        thisBinding: <Global Object>
    }
}
```

__Execution Stage__

```JS
globalExecutionContext:{
    lexicalEnv:{
        environmentRecord:{
            type: "Object",
            a: 1,
            person: <func>,
            sayName: <func> //This will create a new context
        }
        outerEnv: <null>,
        thisBinding: <Global Object>
    },

    variableEnv:{
        environmentRecord:{
            type: "Object",
            b: 2
        },
        outerEnv: <null>,
        thisBinding: <Global Object>
    }
}
```

```JS
functionExecutionContext:{
    lexicalEnv:{
        environmentRecord:{
            type: "Declarative",
            args: {0: "Hello", length: 1},
        }
        outerEnv: <Global Env>,
        thisBinding: <Global Object>
    },

    variableEnv:{
        environmentRecord:{
            type: "Declarative",
        }
        outerEnv: <Global Env>,
        thisBinding: <Global Object>
    }
}
```

Here a Closure will be created because of the inner anonymous function of the `parson` function.

```JS
closure:{
    lexicalEnv:{
        environmentRecord:{
            type: "Declarative",
            args: { length: 0 },
        }
        outerEnv: <Person Env>,
        thisBinding: <Global Object>
    },

    variableEnv:{
        environmentRecord:{
            type: "Declarative",
        }
        outerEnv: <Global Env>,
        thisBinding: <Global Object>
    }
}
```

When the execution stage will come the inner function will get access to the execution context of the outer function through its outerEnv.



