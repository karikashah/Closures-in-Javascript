# Closures-in-Javascript
Lesson plan introducing Closures in JavaScript

## Recap the basic Javascript terminologies
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
A **Local** variable is a function that can access all variables defined **inside** the function. See the example below:
```` javascript
function myFunction() {
  var a = 4;
  return a * a;
} 
````
**a** is a **local** variable.
A local variable can only be used inside the function where it is defined. It is hidden from other functions and other scripting code.

##### *Global variables*
A **Global** variable is a function that can also access variables defined **outside** the function. See the example below:
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
Data privacy is essential for safely sharing code. Without it, anyone using your function/library/framework can maliciously manipulate its inner variables.

Languages like Java and C++ allow classes to have private fields. These fields cannot be accessed outside the class, enabling perfect privacy.
JavaScript doesn't support private variables (yet), **but we can use CLOSURES!**

## The Grand Closure
A closure is a function having access to the parent scope, even after the parent function has closed. 
**Global variables** can be made local (private) with **closures**.

Look at the code snippet below:
```` javascript
  function makeFunc() {
  var name = 'Mozilla';
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc(); // created the instance of makeFunc
myFunc(); // accessing the displayName function 

// the output will be an alert message box with Mozilla.

````
What's different (and interesting) is that the displayName() inner function is returned from the outer function before being executed.
It is extremely important to **return** from the outer function to take the advantage of encapsulation.

## Mapping JavaScript Closure with Object Oriented Concepts:
### 1. Closure functional scope chain
* Objective
* Hand-on application
* Review

### 2. Private methods with closures
Languages such as Java/ C++ allow you to declare methods as private, meaning that they can be called only by other methods in the same class. JavaScript does not provide a native way of doing this, but it is possible to emulate private methods using closures. Private methods aren't just useful for restricting access to code. They also provide a powerful way of managing your global namespace.

The following code illustrates how to use closures to define public functions that can access private functions and variables
```` javascript
function bankAccount() {
	var accountBalance = 0
      return {
          deposit: function(amount) {
              accountBalance += amount;
              alert(accountBalance);
          },
          withdraw: function(amount) {
              // ... safety logic
              accountBalance -= amount;
              alert(accountBalance);
          }
      };
  }
    
  var myBankAccount = bankAccount(); // so initiates the accountBalance to 0 
  myBankAccount.deposit(4000); // sets the value of accountBalance to 4000 & outputs the same in alert message
  myBankAccount.withdraw(2000);// sets the value of accountBalance to 2000 & outputs the same in alert message

````
**NOTE**: In the above example the variable accountBalance can be accessed & its value is modified across both inner functions deposit & withdraw

* Hands-on application
Time to try yourself, 

* Review

### 3. Creating closures in loop
* Objective
* Hand-on application
* Review

## References used
Following sites were referred:
> https://www.w3schools.com/js/js_function_closures.asp

> https://www.freecodecamp.org/news/learn-javascript-closures-in-n-minutes/

> https://www.freecodecamp.org/news/javascript-closure-tutorial-with-js-closure-example-code/

> https://www.tutorialsteacher.com/javascript/closure-in-javascript

> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

