# Learning Extra JavaScript

#### Operators

* Ternatory Operator
* Logical Operators
	* Logical OR - ( || ) It will take the FIRST truthy value or the LAST falsy value. Will return true if either are true, and will return false if both are false. If the first option is already true, JavaScript will not read the second option
	
		```
		var sexy = true || false ---> "true";
		
		```
	* Logical AND - ( && ) Will return true if both parameters are true. false if one of them are false. If both are true, it will return the last value. 
	
		```
		var sexy = true && "damn" ---> "damn";
		```
	* Logical NOT - ( ! ) This is a negation operator which basically stands for NOT. You can use this to alter your statements if needed. 
	
		```
		var sexy = !false && "damn" ---> "damn";
		```
* Short-Circuiting - when not all values in an operator are examined. i.e. var x = true || 0. In here x will be true and the zero is never even looked at.

#### Switch Block

* Switch keyword takes a unique action when checking against a given value
* cases inside the switch
* Fall through- JS will jump to the next cases even if you define a specific case inside the switch. (if you have 10 cases and you call something on case 5, it may still return the value on case 10)
* To stop fall through use the "break" keyword
* Break - makes sure only one case action is taken. It will immediated exit the switch block. If you are calling case 5 it will go to case 5, run that value, then break away from the switch block and will not fall through to case 6


#### Loops
* instead of calling ".length" in the syntax of the for loop you can just make it a variable outside that for loop. (best practices?)
	* you save a lot of memory by "caching" the .length variable 
	* you can also cache your .length variable inside of the for loop as well.
	
	```
		for (i=0, x = array.blah.length, i < x, i++){
			console.log(array.blah[i]);
		}
	```
	* notice you are creating your second variable x (you don't need to call var) and this is cahing the length. 
	* take it one step and cache the object being called in the console.log
	```
		var list = array.blah;
		for (i=0, x=array.blah.length, i<x, i++){
			console.log(list[i])
		}
	```

#### Closures
```
http://javascriptissexy.com/understand-javascript-closures-with-ease/
```

* Closure is an inner function that has access to the variables of the containing function
* A function does not have to return anything in order to be a closure. Accessing variables outside the "inner"functions scope makes that inner function a closure
* Scope chain - closures have access to their own variables, variables in the outer function, and variables in the global scope
* Closures do not have to be functions passed in as parameters. That would be a callback function. Callbacks are closures
* In the example below the console.log function is inside the read function, but is still reaching variables outside of it's scope. 


```
var b = 10;

function read(){
	console.log(b);
}
```

* The inner function makes reference to the variables in the containing function. Even if the containing function has already been called. 


#### Callbacks
```
http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/
```
* Callback functions are Closures
* Callback functions can be named or anonymous
* Review callback funtions and the use of "this"
* A callback function is a function that is used as a parameter or argument in another function. The callback function is not activated immediated, but rather it is called back when a condition in the containing function's body is activated. In the example below the callback function is activated on the click event. 

```
//think of using a method in jQuery on a selected item
$('#button').click(function(){
	console.log('button clicked');
})
```
* Callback functions are closures. closures have access to the containing functions scope, so the callback function can access the containing functions variables, and also the global variables.
* Callbacks can be anonymous or named functions. Instead of using an anonymous function, a user can declare a function with a variable name and pass that name into another functions parameter.
* We can pass parameters to callback functions. Remember since this acts as a closure we can pass any of the containing functions properties or global properties. 

```
var globalvariable = "blah"

function stuff(things, callbackfunction){
	emptyArr.push(things);
	callbackfunction(globalvariable, things)
}
```

* If the callback is not an anonymous function we can make sure the callback is a function before executing it. We do this because if the function is called without a callback function in place it will produce a runtime error. 

```
var globalvariable = "blah"

function stuff(things, callbackfunction){
	emptyArr.push(things);
	if (typeof callbackfunction === "function"){
		callbackfunction(globalvariable, things);
	}
}
```

* Using "this". Remember "this" will grab the window.object over a local object if not targeted properly. 

```
var baseball = {
	team:"",
	setTeamName: function(newTeam){
		this.team = newTeam;
	}
}

function newbaseball(team, callback){
	callback(team)
}

newbaseball(CodLivers, baseball)
console.log(baseball.team) === undefined
console.log(team) === CodLivers (This is in the window/browser)
```
* The above occurs because this.team looked into the window and could not find a team variable so it made a new one. Which is also why the "baseball.team" did not work properly.
* Using "Call" or "Apply" to fix the above example. 
	* Call takes the value to be used as the "this" object inside the containing function
	* Apply functions first parameter is the value to be used as the "this" object

```
function newbaseball(team, callback, callbackObj){
	callback.apply(callbackObj, team);
}

newbaseball(CodLivers, baseball.setTeamName, baseball)

console.log(baseball.team) = CodLivers
```

* Multiple Callback functions can be passed as parameters

#### Scope and Hoisting

```
http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/
```

* variables have either a local scope or global scope
* variables declared in a function are local variables 
* If you do not use "var" in your function the computer will search for a global variable with that name, if there is none it will create one in the global space.

```
var pet = "bunny"

function pets(){
	pet = "dog"
}

console.log(pet) === "bunny"
pets();
console.log(pet) === "dog"
```
* Local variables have priority in functions over global variables

```
var pet = "bunny"

function pets(){
	var pet = "dog";
	console.log(pet);
}
pets(); === "dog"
```
* using a for loop will make a global variable called 'i' that is accessible in other areas

```
for (i = 0; i<10; i++){
	console.log(i); === 1, 2, 3, .....
}

function whatIsI(){
	console.log(i);
}

whatIsI(); === 10
```
* setTimeOut variables are executed in the global scope 

```
var highValue = 200;
​var constantVal = 2;

​var myObj = {
	highValue: 20,
	constantVal: 5,
	calculateIt: function () {
 setTimeout (function  () {
	console.log(this.constantVal * this.highValue);
}, 2000);
	}
}
​
​// The "this" object in the setTimeout function used the global highValue and constantVal variables, because the reference to "this" in the setTimeout function refers to the global window object, not to the myObj object as we might expect.​
​
myObj.calculateIt(); // 400​
```

* variable declarations and function declarations are hoisted to the top. Not their values but their declarations only.

```
function blah(){
	console.log("your pet" + pet);
	var pet = "shadow";
	console.log(pet + "is running");
}
```
* The first console.log will return undefined for pet. The "var pet" is moved to the top of the funtion, but it is not defined. rather the line between the two console logs will read like this (pet = "shadow");
* Function declarations take priority over variable declarations, but not assignments. Variable and Function assignments are not hoisted. 
* Function expressions such as the one below are not hoisted.

```
var thisFunction = function(){
	console.log("banana hammock");
}
```

#### Prototype JavaScript

```
http://javascriptissexy.com/javascript-prototype-in-plain-detailed-language/
```
* Prototype is important because JavaScript does not have classical inheritance like other object oriented languages. In JS you implement inheritance using the Prototype Property.
* Prototype Property
	* Every JavaScript function has a prototype property
	* Properties and methods are attached to the prototype property to create inheritance
	* The prototype property is not enumerable. (cannot be used in for loops)
* Prototype Attribute - normally referred to as the "Prototype Object"
	* The prototype attribute tells us the who/where object's parent is
	* The parent object is where the current object inherited it's properties from
	* This is set every time a new object is created
* Object prototype
	* The object prototype properties are inherited by all JavaScript objects
		* hasOwnProperty()
		* isPrototypeOf()
		* propertyIsEnumerable()
		* toLocaleString()
		* toString()
		* valueOf()
	* All JavaScript Objects will inherit these properties from the Object prototype
	* Object prototypes itself will not inherit any properties
* Other Prototypes
	* String prototype, Array prototype, Number prototype all have their own properties that will be passed down to variables that fall under their designated categories. At the same time these prototypes are inheriting the Object prototype properties as well


#### Objects in JavaScript

```
http://javascriptissexy.com/javascript-objects-in-detail/
```




