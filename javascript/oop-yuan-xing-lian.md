# Objects, Classes, and Object-Oriented Programming

## Understanding Objects:

###  **D**ata properties:

* Configurable
  * **Indicates if the property may be redefined by removing the property**

    **via delete, changing the property’s attributes,** or changing the property into an accessor

    property. By default, this is true for all properties defined directly on an object

  * **Calling delete on the property has no effect in nonstrict mode and throws an error in strict mode.**
* Enumerable
  * **Indicates if the property will be returned in a for-in loop**. By default,

    this is true for all properties defined directly on an object
* Writable
  * **Indicates if the property’s value can be changed**. By default, this is true for

    all properties defined directly on an object
* Value
  * **Contains the actual data value for the property**. This is the location from which

    the property’s value is read and the location to which new values are saved

```text
let person = {
 name: "Nicholas"
};

```

* Here, the property called name is created and a value of "Nicholas" is assigned. That means

  \[\[Value\]\] is set to "Nicholas", and any changes to that value are stored in this location.

To change any of the default property attributes you must use the **`Object.defineProperty()`**

```text
let person = {
  name:'boyan'
};
console.log(person);//boyan
Object.defineProperty(person, "name", {
 writable: false,
 value: "Nicholas"
});
console.log(person.name); // "Nicholas"
person.name = "Greg";
console.log(person.name); // "Nicholas"
```

* . This method accepts three arguments: the object on which the property should be added or

  modified, the name of the property, and a descriptor object. The properties on the descriptor object

  match the attribute names: configurable, enumerable, writable, and value. You can set one or all

  of these values to change the corresponding attribute values

* **Once setting configurable to false means that the property cannot be removed from the object. It can not changed configurable any more**

### **Accessor properties**

* Accessor properties do not contain a data value. Instead, they contain a combination of a getter function and a setter function \(though both are not necessary\)。
* Accessor properties have four attributes:
  * Configurable
  * Enumerable
  * Get—The function to call when the property is read from. The default value is undefined
  * Set—The function to call when the property is written to. The default value is **undefined**

```text
// Define object with pseudo-private member 'year_'
// and public member 'edition'
let book = {
 year_: 2017,
 edition: 1
};
console.log(book.year);  // undefined
Object.defineProperty(book, "year", {
 get() {
 return this.year_;
 },
 set(newValue) {
 if (newValue > 2017) {
 this.year_ = newValue;
 this.edition += newValue - 2017;
 }
 }
});
book.year = 2018;
console.log(book.edition); // 2
console.log(book.year); // 2018
```

### Defining multiple properties

```text
let book = {};
Object.defineProperties(book, {
 year_: {
 value: 2017
 },

 edition: {
 value: 1
 },

 year: {
 get() {
 return this.year_;
 },

 set(newValue) {
 if (newValue > 2017) {
 this.year_ = newValue;
 this.edition += newValue - 2017;
 }
 }
 }
});
```

300





