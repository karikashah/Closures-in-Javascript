# Closures-in-Javascript
Closures are important in functional programming, and are often asked during the JavaScript coding interview.
While being used everywhere, closures are difficult to grasp. If you haven’t had your “Aha!” moment in understanding closures, then this lesson plan is right for you.

You will start with the fundamental terms: functions, variables and lexical scope. Then, after grasping the basics, you’ll enter the closures & dive into the concepts mapping with OOP (Object Oriented Programming).

Before starting, I suggest you resist the urge to skip the fundamental JavaScript terms sections. These concepts are crucial to closures, and if you get them well, the idea of closure becomes self-evident. 

## Fundamental JavaScript terms
#### Functions
A JavaScript function is a block of code designed to perform a particular task. A JavaScript function is executed when "something" invokes it (calls it).
A JavaScript function is defined with the function keyword, followed by a name, followed by parentheses ().

Function names can contain letters, digits, underscores, and dollar signs (same rules as variables).

The parentheses may include parameter names separated by commas:
(parameter1, parameter2, ...)

The code to be executed, by the function, is placed inside curly brackets: {}
```` javascript
function name(parameter1, parameter2, parameter3) {
  // code to be executed
}
````
#### Variables
JavaScript variables can belong to the **local** or **global** scope. 
##### *Local variables*
A **Local** variable: a function that can access all variables defined **inside** the function. See the example below:
```` javascript
function myFunction() {
  var a = 4;
  return a * a;
} 
````
**a** is a **local** variable.
A local variable can only be used inside the function where it is defined. It is hidden from other functions and other scripting code.

##### *Global variables*
A **Global** variable: a function that can also access variables defined **outside** the function. See the example below:
```` javascript
var a = 4;
function myFunction() {
  return a * a;
} 
````
**a** is a **global** variable.
In a web page, global variables belong to the window object.
Global variables can be used (and changed) by all scripts in the page (and in the window). They live until the page is discarded, like when you navigate to another page or close the window.

* Global and local variables with the same name are different variables. Modifying one, does not modify the other.
* Variables created without a declaration keyword **(var, let, or const)** are always **global**, even if they are created inside a function.

#### Lexical Scoping
Lexical scoping is how a parser resolves variable names when functions are nested. The word lexical refers to the fact that lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available. Nested functions have access to variables declared in their outer scope. Consider the following example code:
```` javascript
function main() {
  var name = 'Hello World'; // name is a local variable created by main function
  function displayName() { // displayName() is the inner function
    alert(name); // use variable declared in the parent function
  }
  displayName();
}
main();

````
Run the code using this [JSFiddle link](http://jsfiddle.net/2bzup8fk/1/) and notice that the alert() statement within the displayName() function successfully displays the value of the name variable, which is declared in its parent function.

## A Dilemma
Here is the code snippet to make deposit or withdraw in the bank account. You could use a global variable, and a *function* to deposit or withdraw from the bank account. 

```` javascript
  let accountBalance = 0;
  const manageBankAccount = function() {
      return {
          deposit: function(amount) {
              accountBalance += amount;
          },
          withdraw: function(amount) {
              // ... safety logic
              accountBalance -= amount;
          }
      };
  }
````
The issue is the accountBalance is a global variable & is *EXPOSED* to all the functions!
What's stopping me from inflating my balance or ruining someone else's?
```` javascript
 // later in the script...
 accountBalance = 'Whatever I want! :)';
`````
**Data privacy is essential for safely sharing code.** Without it, anyone using your function/library/framework can maliciously manipulate its inner variables.

Languages like Java and C++ allow classes to have private fields. These fields cannot be accessed outside the class, enabling perfect privacy.
JavaScript doesn't support private variables (yet), **but we can use CLOSURES!**

## The Grand Closure
A closure is a function having access to the parent scope, even after the parent function has closed. 
**Global variables** can be made local (private) with **closures**.

Look at the code snippet below:
```` javascript
  function main() {
  var name = 'Hello World'; // name is a local variable created by main function
  function displayName() {
    alert(name); // use variable declared in the parent function
  }
  return displayName;
}

var myFunc = main(); // created the instance of main
myFunc(); // accessing the displayName function 

// the output will be an alert message box with 'Hello World'

````
What's different (and interesting) is that the displayName() inner function is returned from the outer function before being executed.
It is extremely important to **return** from the outer function to take the advantage of [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)) (referred to the bundling of data with the methods that operate on that data, or the restricting of direct access to some of an object's components).

## Deep dive in Closure

Let us map the JavaScript Closure concept with the Object Oriented Concepts used by many popular languages like Java & C++:
### 1. Closure functional scope chain
Every closure has three scope chains:

    it has access to its *own scope* — variables defined between its curly brackets
    it has access to the *outer function’s* variables
    it has access to the *global* variables

To demonstrate, consider the following example code.
```` javascript
  // global scope
  let accountBalance = 0;
  
function bankAccount(accountName) {
	// outer functions scope
	alert(accountName); // alerts with message "John Charles"
	return {
		deposit: function(amount) {
		accountBalance += amount;
		alert(accountName + " " + accountBalance);
	     },
		withdraw: function(amount) {
		// local scope
		accountBalance -= amount;
		alert(accountName + " " + accountBalance);
	     }
	};
  }
  var myBankAccount = bankAccount("John Charles"); // display alert dialog box
  myBankAccount.deposit(2000); // should display alert box with account name & deposited amount
````
In the above example:
1. *accountBalance* is the global variable
2. *amount* is the local variable for *deposit()* function where as *accountName* is the outer function's variable i.e. local variable of outer function *bankAccount()* function.

Run the code using this [JSFiddle link](http://jsfiddle.net/2bzup8fk/2/) and notice that the alert() statement within the *deposit()* function successfully displays the value of the *accountName* variable (declared in outer function scope) and *accountBalance* variable (declared in global scope).

A common mistake is not realizing that, in the case where the outer function is itself a nested function, access to the outer function's scope includes the enclosing scope of the outer function—effectively creating a chain of function scopes.

* **Hands-on application: Event Handler**

Prepare a small code snippet with textbox & button. Each time when you click the button, the text updates to show the number of clicks. 

*Hint*: You can use the closure function specified below:
````javascript
let countClicked = 0;

myButton.addEventListener('click', function handleClick() {
  countClicked++;
  myText.innerText = `You clicked ${countClicked} times`;
});
````

* **Review**

	* Closure has 3 scopes - global, local & outer function scope
	* The scope is what rules the accessibility of variables in JavaScript. 
	* The lexical scope allows a function scope to access statically the variables from the outer scopes. 

### 2. Private variables & methods with closures
Languages such as Java/ C++ allow you to declare methods as private, meaning that they can be called only by other methods in the same class. JavaScript does not provide a native way of doing this, but it is possible to emulate private methods using closures. Private methods aren't just useful for restricting access to code. They also provide a powerful way of managing your global namespace.

The following code illustrates how to use closures to define public functions (outer function) that can access private functions (inner functions) and variables
```` javascript
function bankAccount() { // outer function)
	var accountBalance = 0
      return {
          deposit: function(amount) { // inner function
              accountBalance += amount;
              alert('After the deposit of '+ amount + ' the balance is ' + accountBalance + '.');
          },
          withdraw: function(amount) {
              // ... safety logic
              accountBalance -= amount;
              alert('After the withdrawal of '+ amount + ' the balance is ' + accountBalance + '.');
          }
      };
  }
    
  var myBankAccount = bankAccount(); // so initiates the accountBalance to 0 
  myBankAccount.deposit(4000); // sets the value of accountBalance to 4000 & outputs the same in alert message
  myBankAccount.deposit(6000); // sets the value of accountBalance to 10,000 
  myBankAccount.withdraw(2000);// sets the value of accountBalance to 8000 & outputs the same in alert message

````
Run the code using this [JSFiddle link](http://jsfiddle.net/komgj259/1/)
**NOTE**: In the above example the variable accountBalance can be accessed & its value can be modified by both inner functions - deposit() & withdraw()

* **Hands-on application: **

Prepare a code snippet to track of top 5 technological stocks. For each stock, maintain following information: Ticker symbol, price, earnings per share, dividend, & dividend yield. Based on the stock information, suggest the best stock to purchase & why? (*NOTE*: You can display alert/console.log giving reason to support your suggestion)    

* **Review**

	* In many object-oriented programming languages, there is a way to limit the visibility of a variable from outside its scope. The closure remembers the variables from the place where it is defined, no matter where it is executed.
	* It is unwise to unnecessarily create functions within other functions if closures are not needed for a particular task, as it will negatively affect script performance both in terms of processing speed and memory consumption.

### 3. Currying: Functional programming
Currying is a process in functional programming in which we can transform a function with multiple arguments into a sequence of nesting functions. It returns a new function that expects the next argument inline.
It keeps returning a new function until all the arguments are exhausted. The arguments are kept "alive"(via closure) and all are used in execution when the final function in the currying chain is returned and executed.

For example:
````javascript
// simple multiplication example, takes in 3 arguments:
function multiply(a, b, c) {
    return a * b * c;
}

const total = multiply (1,2,3);
alert(total); // will return 6

// curried multiplication example, takes one argument at a time
function multiply(a) {
    return (b) => {
        return (c) => {
            return a * b * c
        }
    }
}

const total = multiply(1)(2)(3) // will return 6

````
Run the curried code using this [JSFiddle link](http://jsfiddle.net/komgj259/2/)
We have turned the *multiply(1,2,3)* function call to *multiply(1)(2)(3)* multiple function calls.

One single function has been turned to a series of functions. To get the result of multiplication of the three numbers 1, 2 and 3, the numbers are passed one after the other, each number prefilling the next function inline for invocation.

* **Hand-on application**

You own a store and you want to give 10% discount to your fav customers who makes their purchase over $500. Create a curried discount function for the fav customer.
Now, we have super-fav customers who makes their purchases between $500 - $10,000. We want to give 20% discount to our super-fav customers. Create a new curried discount function to handle that usecase.

* **Review**

	* Closure makes currying possible in JavaScript.
	* A curried function is a function that takes multiple (more than two) arguments one at a time.
	* Currying happens when a function returns another function until the arguments are fully supplied. 
	* With curried functions you get easier reuse of more abstract functions, since you get to specialize.

## References used
Following sites were referred:
> https://www.w3schools.com/js/js_function_closures.asp

> https://www.freecodecamp.org/news/learn-javascript-closures-in-n-minutes/

> https://www.freecodecamp.org/news/javascript-closure-tutorial-with-js-closure-example-code/

> https://www.tutorialsteacher.com/javascript/closure-in-javascript

> https://dmitripavlutin.com/simple-explanation-of-javascript-closures/

> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

> https://blog.bitsrc.io/understanding-currying-in-javascript-ceb2188c339

