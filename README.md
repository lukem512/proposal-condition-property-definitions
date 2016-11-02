# Condition Property Definitions for ECMAScript
This proposal introduces an operator for defining conditional properties in ECMAScript objects. This operator allows properties to be included if a specific condition evaluates to true.

The operator can be used to add a single property subject to a condition.

```js
let obj = {
  cond ? prop: value
};
```

The operator can also be used to add multiple properties subject to a single condition; this is achieved by declaring the properties within a block.

```js
let obj = {
  cond ? {
    prop1: value1,
    prop2: value2
  }
};
```

Multiple conditions can also be combined within parenthesis to create more complex conditions for including properties.

```js
let obj = {
  (cond1 && cond2) ? prop: value
};
```

## Motivation for Use

The use case for this operator is that there is often a need to add a property if and only if a certain condition is satisfied. This requires the object to be assigned to a variable and for a conditional statement to be used, resulting in several additional lines of code.

```js
function(cond) {
  let obj = {
    x: 14,
    y: 39
  };
  if (cond) {
    obj.prop = true;
  }
  return obj;
}
```

By using the operator proposed in this document, the function definition can be simplified.

```js
function(cond) {
  return {
    x: 14,
    y: 39,
    cond ? prop: true
  };
}
``` 

## Existing Solutions

This behaviour can sometimes be emulated using a ternary operator with the false expression set to `undefined`. This workout cannot be used in certain situations however, such as when performing operation an update on a database, as the property is still present in the object. Certain applications may iterate over the keys within an object, using `Object.keys` or a similar function, and take undesirable action based upon this.

```js
function(cond) {
  return {
    x: 14,
    y: 39,
    prop: cond ? true : undefined
  };
}
``` 
