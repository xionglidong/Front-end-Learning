### Detect Data Type
As we all know, JavaScript data types are divided into original data types and reference data types. But do you know how to accurately detect the data type?

Here we introduce three methods to detect the data type. 

### typeof
The `typeof` is the most commonly used method in our daily work.It returns a string indicating the type of the operand's value.

###### Try it
```JavaScript
console.log(typeof 42);
// Expected output: "number"

console.log(typeof 'blubber');
// Expected output: "string"

console.log(typeof true);
// Expected output: "boolean"

console.log(typeof undeclaredVariable);
// Expected output: "undefined"
```

The following table summarizes the possible return values of `typeof`.


| Type | Result |
| --- | --- |
| Undefined | "undefined" |
| Null | "object" |
| Boolean | "boolean" |
| Number | "number" |
| BigInt | "bigint" |
| String | "string" |
| Symbol | "symbol" |
| Function | "function" |
| Any other object | "object" |

Perhaps you are curious why the result of typeof Null is equal to the "object".The explanation is as follows:

*In the first implementation of JavaScript, JavaScript values were represented as a type tag and a value. The type tag for objects was `0`. `null` was represented as the NULL pointer (`0x00` in most platforms). Consequently, `null` had `0` as type tag, hence the `typeof` return value `"object"`.*

From this,we can see that the `typeof` can accurately detect the original data type,but it's not suitable for detecting reference data types.

### instance of

The **`instanceof`** operator tests to see if the `prototype` property of a constructor appears anywhere in the prototype chain of an object. The return value is a boolean value.

###### Try it
```JavaScript
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
const auto = new Car('Honda', 'Accord', 1998);

console.log(auto instanceof Car);
// Expected output: true

console.log(auto instanceof Object);
// Expected output: true
```

We can implement it ourselves to help us better understand its principle.
```JavaScript
function myInstanceof(object, constructor) {
  //Obtain the prototype of the object
  let proto = Object.getPrototypeOf(object)
  //Get the prototype object of the constructor
  let prototype = constructor.prototype; 
 
  //Determine whether the prototype object of the constructor is on the prototype chain of the object
  while (true) {
    if (!proto) return false;
    if (proto === prototype) return true;
    //If not found, continue to search for its prototype. The Object. getPrototypeOf method is used to obtain the prototype of the specified object
    proto = Object.getPrototypeOf(proto);
  }
}
```
So, if we want to detect a reference type, `instanceof` would be a good method.But it can't detect the original data type.

### Object.prototype.toString.call()

`Object.prototype.toString()` returns `"[object Type]"`, where `Type` is the object type. If the object has a `Symbol.toStringTag` property whose value is a string, that value will be used as the `Type`. Many built-in objects, including `Map`and `Symbol`, have a `Symbol.toStringTag`. Some objects predating ES6 do not have `Symbol.toStringTag`, but have a special tag nonetheless.

To use the base `Object.prototype.toString()` with an object that has it overridden (or to invoke it on `null` or `undefined`), you need to call `Function.prototype.call()` or `Function.prototype.apply()` on it, passing the object you want to inspect as the first parameter (called `thisArg`).

```JavaScript
const arr = [1, 2, 3];

arr.toString(); // "1,2,3"
Object.prototype.toString.call(arr); // "[object Array]"
```
We found this method can accurately detect both original and reference data types.
We should choose the most suitable method based on actual situation in daily work.

Anyway i hope it helps for you, see you next time, bye bye.
