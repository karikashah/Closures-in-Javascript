# Closures-in-Javascript
Lesson plan introducing Closures in JavaScript

## Recap the basic Javascript terminologies
#### 1. Functions
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
#### 2. Variables
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

## Closure - its purpose
A closure is a function having access to the parent scope, even after the parent function has closed. 
**Global variables** can be made local (private) with **closures**.

Look at the code snippet below:
```` javascript
  var add = (function () {
  var counter = 0;
  return function () {counter += 1; return counter}
})();

add();
add();
add();
add();
add();
// the counter is now 5 
````
The variable add is assigned to the return value of a self-invoking function. The self-invoking function only runs once. It sets the counter to zero (0), and returns a function expression. This way add becomes a function. The "*wonderful*" part is that it can access the counter in the parent scope. This is called a **JavaScript closure**. It makes it possible for a function to have "*private*" variables. The counter is protected by the scope of the anonymous function and can only be changed using the add function.
