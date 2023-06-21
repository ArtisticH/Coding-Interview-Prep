# Algorithms

### Find the Symmetric Difference
The mathematical term symmetric difference (`△` or `⊕`) of two sets is the set of elements which are in either of the two sets but not in both.  
For example, for sets `A = {1, 2, 3}` and `B = {2, 3, 4}`, `A △ B = {1, 4}`.  
Symmetric difference is a binary operation, which means it operates on only two elements.  
So to evaluate an expression involving symmetric differences among three elements (`A △ B △ C`), you must complete one operation at a time.  
Thus, for sets `A` and `B` above, and `C = {2, 3}`, `A △ B △ C = (A △ B) △ C = {1, 4} △ {2, 3} = {1, 2, 3, 4}`.  
  
Create a function that takes two or more arrays and returns an array of their symmetric difference.  
The returned array must contain only unique values (no duplicates).
```
function sym(args) {
 args = [...arguments];
 
 function two(arg1, arg2) {
   let one = arg1.filter(item => !arg2.includes(item));
   let two = arg2.filter(item => !arg1.includes(item));
   let noDuplicates = one.concat(two).filter((item, i, arr) => arr.indexOf(item) === i).sort((a, b) => a === b ? 0 : a < b ? -1 : 1);
   return noDuplicates;
 }
 return args.reduce((acc, cur) => two(acc, cur));
}

sym([1, 2, 3], [5, 2, 1, 4]);
sym([1, 2, 3, 3], [5, 2, 1, 4]);
sym([1, 2, 3], [5, 2, 1, 4, 5]);

sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5]);
sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5]);
sym([3, 3, 3, 2, 5], [2, 1, 5, 7], [3, 4, 6, 6], [1, 2, 3]);
sym([3, 3, 3, 2, 5], [2, 1, 5, 7], [3, 4, 6, 6], [1, 2, 3], [5, 3, 9, 8], [1]);
```
👇 Solution 1
```
function sym() {
  const args = [];
  for (let i = 0; i < arguments.length; i++) {
    args.push(arguments[i]);
  }

  function symDiff(arrayOne, arrayTwo) {
    const result = [];

    arrayOne.forEach(function (item) {
      if (arrayTwo.indexOf(item) < 0 && result.indexOf(item) < 0) {
        result.push(item);
      }
    });

    arrayTwo.forEach(function (item) {
      if (arrayOne.indexOf(item) < 0 && result.indexOf(item) < 0) {
        result.push(item);
      }
    });

    return result;
  }

  // Apply reduce method to args array, using the symDiff function
  return args.reduce(symDiff);
}
```
👇 Solution 2
```
function sym(...args) {
  // Return the symmetric difference of 2 arrays
  const getDiff = (arr1, arr2) => {
    // Returns items in arr1 that don't exist in arr2
    function filterFunction(arr1, arr2) {
      return arr1.filter(item => arr2.indexOf(item) === -1);
    }

    // Run filter function on each array against the other
    return filterFunction(arr1, arr2).concat(filterFunction(arr2, arr1));
  };

  // Reduce all arguments getting the difference of them
  const summary = args.reduce(getDiff, []);

  // Run filter function to get the unique values
  const unique = summary.filter((elem, index, arr) => index === arr.indexOf(elem));
  return unique;
}
```

### Inventory Update
