# Algorithms

### Find the Symmetric Difference
The mathematical term symmetric difference (`△` or `⊕`) of two sets is the set of elements which are in either of the two sets but not in both.For example, for sets `A = {1, 2, 3}` and `B = {2, 3, 4}`, `A △ B = {1, 4}`. Symmetric difference is a binary operation, which means it operates on only two elements.So to evaluate an expression involving symmetric differences among three elements (`A △ B △ C`), you must complete one operation at a time.Thus, for sets `A` and `B` above, and `C = {2, 3}`, `A △ B △ C = (A △ B) △ C = {1, 4} △ {2, 3} = {1, 2, 3, 4}`.  
  
Create a function that takes two or more arrays and returns an array of their symmetric difference. The returned array must contain only unique values (no duplicates).
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
Compare and update the inventory stored in a 2D array against a second 2D array of a fresh delivery. Update the current existing inventory item quantities (in `arr1`). If an item cannot be found, add the new item and quantity into the inventory array. The returned inventory array should be in alphabetical order by item.
```
function updateInventory(arr1, arr2) {
    let forUpdate = [];
    let forAdd = [];
    let justName = arr1.map(item => item[1]);
    // arr2 split to update and add
    arr2.forEach(item => justName.includes(item[1]) 
                            ? forUpdate.push(item)
                            : forAdd.push(item));
    // UPDATE
    let justName_update = forUpdate.map(item => item[1]);
    arr1 = arr1.map(item => {
        if(justName_update.includes(item[1])) {
            let index = justName_update.indexOf(item[1]);
            let sum = item[0] + forUpdate[index][0];
            return [sum, item[1]];
        } else {
            return item;
        }
    })
    // ADD
    arr1 = arr1.concat(forAdd);
    console.log(arr1.sort((a, b) => a[1] === b[1] ? 0 : a[1] < b[1] ? -1 : 1));
    return arr1.sort((a, b) => a[1] === b[1] ? 0 : a[1] < b[1] ? -1 : 1);
}


updateInventory([[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]], [[2, "Hair Pin"], [3, "Half-Eaten Apple"], [67, "Bowling Ball"], [7, "Toothpaste"]])
updateInventory([[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]], []);
updateInventory([], [[2, "Hair Pin"], [3, "Half-Eaten Apple"], [67, "Bowling Ball"], [7, "Toothpaste"]]);
updateInventory([[0, "Bowling Ball"], [0, "Dirty Sock"], [0, "Hair Pin"], [0, "Microphone"]], [[1, "Hair Pin"], [1, "Half-Eaten Apple"], [1, "Bowling Ball"], [1, "Toothpaste"]]);
```
👇 Solution 1
```
function updateInventory(arr1, arr2) {
  // Variable for location of product
  var index;

  // A helper method to return the index of a specified product (undefined if not found)
  var getProductIndex = function(name) {
    for (var i = 0; i < this.length; i++) {
      if (this[i][1] === name) {
        return i;
      }
    }
    return undefined;
  };

  // For each item of the new Inventory
  for (var i = 0; i < arr2.length; i++) {
    // Invoke our helper function using arr1 as this
    index = getProductIndex.call(arr1, arr2[i][1]);

    // If the item doesn't exist
    if (index === undefined) {
      // Push the entire item
      arr1.push(arr2[i]);
    } else {
      // Add the new quantity of the current item
      arr1[index][0] += arr2[i][0];
    }
  }

  // Sort alphabetically, by the product name of each item
  arr1.sort(function(a, b) {
    if (a[1] > b[1]) {
      return 1;
    }
    if (a[1] < b[1]) {
      return -1;
    }
    return 0;
  });

  return arr1;
}

// test here
// Example inventory lists
var curInv = [
  [21, "Bowling Ball"],
  [2, "Dirty Sock"],
  [1, "Hair Pin"],
  [5, "Microphone"]
];

var newInv = [
  [2, "Hair Pin"],
  [3, "Half-Eaten Apple"],
  [67, "Bowling Ball"],
  [7, "Toothpaste"]
];

updateInventory(curInv, newInv);
```
👇 Solution 2
```
function updateInventory(arr1, arr2) {
  // All inventory must be accounted for or you're fired!

  var index;
  var arrCurInvName = []; // Names of arr1's items
  var arrNeInvName = []; // Names of arr2's items

  // Same as using two for loops, this takes care of increasing the number of stock quantity.
  arr1.forEach(function(item1) {
    arr2.forEach(function(item2) {
      if (item1[1] === item2[1]) {
        item1[0] = item1[0] + item2[0]; //Increase number of stock
      }
    });
  });

  // Get item's name for new Inventory
  arr2.forEach(function(item) {
    arrNeInvName.push(item[1]);
  });

  // Get item's name for Current Inventory
  arr1.forEach(function(item) {
    arrCurInvName.push(item[1]);
  });

  // Add new inventory items to current inventory.
  arrNeInvName.forEach(function(item) {
    if (arrCurInvName.indexOf(item) === -1) {
      index = arrNeInvName.indexOf(item);
      arr1.push(arr2[index]);
    }
  });

  // Sort the array alphabetically using the second element of the array as base.
  arr1.sort(function(currItem, nextItem) {
    //Ternary function to avoid using if else
    return currItem[1] > nextItem[1] ? 1 : -1;
  });

  return arr1;
}

// test here
// Example inventory lists
var curInv = [
  [21, "Bowling Ball"],
  [2, "Dirty Sock"],
  [1, "Hair Pin"],
  [5, "Microphone"]
];

var newInv = [
  [2, "Hair Pin"],
  [3, "Half-Eaten Apple"],
  [67, "Bowling Ball"],
  [7, "Toothpaste"]
];

updateInventory(curInv, newInv);
```

### No Repeats Please
Return the number of total permutations of the provided string that don't have repeated consecutive letters. Assume that all characters in the provided string are each unique.

For example, `aab` should return 2 because it has 6 total permutations (`aab`, `aab`, `aba`, `aba`, `baa`, `baa`), but only 2 of them (`aba` and `aba`) don't have the same letter (in this case a) repeating.
```
function permAlone(str) {
  return str;
}

permAlone('aab');


// console.log(permutation('aab'));
// console.log(permutation('abc'));
// console.log(permutation("aaa"));

function permutation(str) {
  str = [...str];

  let permu = str.map((item, i, arr) => {
    let index = i;
    let slice = arr.filter((v, i) => i !== index);
    return [[item].concat(slice), [item].concat(slice.reverse())];
  }).flat();
  console.log(permu);
  let answer = permu.filter((item) => {
    let count = 0;
    for(let j = 0; j < item.length; j++) {
      if(item[j] === item[j+1]) count++;
    }
    return count > 0 ? false : true;
  })
  console.log(answer);
  return answer.length;
}

```

