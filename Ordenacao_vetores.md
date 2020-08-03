Definition and Usage
The sort() method sorts the items of an array.

The sort order can be either alphabetic or numeric, and either ascending (up) or descending (down).

By default, the sort() method sorts the values as strings in alphabetical and ascending order.

This works well for strings ("Apple" comes before "Banana"). However, if numbers are sorted as strings, "25" is bigger than "100", because "2" is bigger than "1".

Because of this, the sort() method will produce an incorrect result when sorting numbers.

You can fix this by providing a "compare function" (See "Parameter Values" below).

Note: This method changes the original array.

Syntax
array.sort(compareFunction)
Parameter Values
Parameter	Description
compareFunction	Optional. A function that defines an alternative sort order. The function should return a negative, zero, or positive value, depending on the arguments, like:
function(a, b){return a-b}
When the sort() method compares two values, it sends the values to the compare function, and sorts the values according to the returned (negative, zero, positive) value.

Example:

When comparing 40 and 100, the sort() method calls the compare function(40,100).

The function calculates 40-100, and returns -60 (a negative value).

The sort function will sort 40 as a value lower than 100.
By default, the JavaScript Array.sort function converts each element in the array that needs to be sorted into a string, and compares them in Unicode code point order.
You may be wondering why 32 comes before 5. Not logical, huh? Well, actually it is. This happens because each element in the array is first converted to a string, and "32" comes before "5" in Unicode order.



```Javascript
const foo = [9, 1, 4, 'zebroid', 'afterdeck'];
foo.sort(); // returns [ 1, 4, 9, 'afterdeck', 'zebroid' ]

const bar = [5, 18, 32, new Set, { user: 'Eleanor Roosevelt' }];
bar.sort(); // returns [ 18, 32, 5, { user: 'Eleanor Roosevelt' }, Set {} ]

```
It’s also worth noting that unlike many other JavaScript array functions, Array.sort actually changes, or mutates the array it sorts.
```Javascript
const baz = ['My cat ate my homework', 37, 9, 5, 17];
baz.sort(); // baz array is modified
console.log(baz); // shows [ 17, 37, 5, 9, 'My cat ate my homework' ]
```
To avoid this, you can create a new instance of the array to be sorted and modify that instead. This is possible using an array method that returns a copy of the array. For example, Array.slice:
```Javascript
const sortedBaz = baz.slice().sort(); // a new instance of the baz array is created and sorted
```
r if you prefer a newer syntax, you can use the spread operator for the same effect:
```Javascript
const sortedBaz = [...baz].sort(); // a new instance of the baz array is created and sorted
```
# Using Compare Functions to Sort
Let’s say that foo and bar are the two elements being compared by the compare function, and the return value of the compare function is set up as follows:

less than 0 — foo comes before bar
greater than 0  — bar comes before foo
equal to 0  — foo and bar are left unchanged with respect to each other.
Let’s look at a simple example with an array of numbers:
```Javascript
const nums = [79, 48, 12, 4];

function compare(a, b) {
  if (a > b) return 1;
  if (b > a) return -1;

  return 0;
}

nums.sort(compare);
// => 4, 12, 48, 79
```
This is now a good candidate for an arrow function:
```Javascript
nums.sort((a, b) => a - b);
```


Ordenando Vetor utilizando função de comparação 
```Javascript
const singers = [
  { name: 'Steven Tyler', band: 'Aerosmith', born: 1948 },
  { name: 'Karen Carpenter', band: 'The Carpenters', born: 1950 },
  { name: 'Kurt Cobain', band: 'Nirvana', born: 1967 },
  { name: 'Stevie Nicks', band: 'Fleetwood Mac', born: 1948 },
];

function compareValues(key, order = 'asc') {
  return function innerSort(a, b) {
    if (!a.hasOwnProperty(key) || !b.hasOwnProperty(key)) {
      // property doesn't exist on either object
      return 0;
    }

    const varA = (typeof a[key] === 'string')
      ? a[key].toUpperCase() : a[key];
    const varB = (typeof b[key] === 'string')
      ? b[key].toUpperCase() : b[key];

    let comparison = 0;
    if (varA > varB) {
      comparison = 1;
    } else if (varA < varB) {
      comparison = -1;
    }
    return (
      (order === 'desc') ? (comparison * -1) : comparison
    );
  };
}
```
To reverse the sorting order, you can invert the return value of the compare function:

```Javascript
function compare(a, b) {
  ...

  //invert return value by multiplying by -1
  return comparison * -1;
}
```
```Javascript
```
```Javascript
```
