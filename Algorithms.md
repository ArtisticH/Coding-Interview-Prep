# Algorithms

hese free programming exercises will teach you about some common algorithms that you will likely encounter in real life. They are a great opportunity to improve your logic and programming skills.

These algorithms are frequently used in job interviews to test a candidate's skills. We'll give you clear and concise explanations of how these different algorithms work so you can implement a solution for each one.

## Find the Symmetric Difference

The mathematical term symmetric difference (`△` or `⊕`) of two sets is the set of elements which are in either of the two sets but not in both. For example, for sets `A = {1, 2, 3} `and `B = {2, 3, 4}`, `A △ B = {1, 4}`.

Symmetric difference is a binary operation, which means it operates on only two elements. So to evaluate an expression involving symmetric differences among three elements (`A △ B △ C`), you must complete one operation at a time. Thus, for sets A and B above, and `C = {2, 3}`, `A △ B △ C = (A △ B) △ C = {1, 4} △ {2, 3} = {1, 2, 3, 4}`.  

Create a function that takes two or more arrays and returns an array of their symmetric difference. The returned array must contain only unique values (no duplicates).

📝
- 인수의 첫 번째, 두 번째를 정해놓고 나머지는 나머지로 치부
- 두 가지 배열에서 교집합을 제외. 만약 a의 요소가 b에도 제외. 즉 a의 요소 중에서 b에 속하지 않은 것만 골라내기
- 두 배열을 결합하고 오름차순으로 정리
- 중복되는 요소 제거
- 만약 인수가 2개 보다 많다면, 재귀 호출.
- 그래서 인수가 2개일 때까지 호출하다가 2개가 된다면, 마지막 중복 제거된 배열 반환


```javascript
function sym(args) {
  let [arr1, arr2, ...rest] = arguments;
  arr1 = arr1.filter(item => !arguments[1].includes(item));
  arr2 = arr2.filter(item => !arguments[0].includes(item));
  const combine = arr1.concat(arr2).sort();

  // 중복 제거
  const removeDuple = combine.filter((item, index, arr) => {
    return arr.indexOf(item) === index;
  })

  if(rest.length > 0) return sym(removeDuple, ...rest);
  if(rest.length === 0) return removeDuple;
}

console.log(sym([1, 2, 3], [5, 2, 1, 4])); // [3, 4, 5]
console.log(sym([1, 2, 3, 3], [5, 2, 1, 4])); // [3, 4, 5]
console.log(sym([1, 2, 3], [5, 2, 1, 4, 5])); // [3, 4, 5]
console.log(sym([1, 2, 5], [2, 3, 5], [3, 4, 5])); // [ 1, 4, 5 ]
console.log(sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5])); // [ 1, 4, 5 ]
console.log(sym([3, 3, 3, 2, 5], [2, 1, 5, 7], [3, 4, 6, 6], [1, 2, 3])); // [ 2, 3, 4, 6, 7 ]
console.log(sym([3, 3, 3, 2, 5], [2, 1, 5, 7], [3, 4, 6, 6], [1, 2, 3], [5, 3, 9, 8], [1])); // [ 1, 2, 4, 5, 6, 7, 8, 9 ]
```

🔐 solution1
```javascript
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

- 인수 배열에 대해 reduce 메서드
- arrayOne 요소 중에 arrayTwo에 속하지 않고 중복되지 않는 것만 추가.
- arrayTwo도 마찬가지.
- 하나의 결과값 result


## Inventory Update

Compare and update the inventory stored in a 2D array against a second 2D array of a fresh delivery. Update the current existing inventory item quantities (in arr1). If an item cannot be found, add the new item and quantity into the inventory array. The returned inventory array should be in alphabetical order by item.
  
  
📝  
- new 배열 요소 하나가 cur 배열 전체를 순회하며 같은 문자열 값이 있는지 검사
- 있다면, `existed = true`하고, 숫자 값 add
- 없다면, `existed = false`라면 cur 배열에 추가
- 마지막으로 문자열 오름차순


```javascript
function updateInventory(arr1, arr2) {
    arr2.forEach((item_new, index_new, arr_new) => {
        let existed = false;
        arr1.forEach((item_cur, index_cur, arr_cur) => {
            if(item_cur.includes(item_new[1])) {
                item_cur[0] += item_new[0];
                existed = true;
                return;
            }
        })
        if(!existed) {
            arr1.push(item_new);
        }
    })
    return arr1.sort((a, b) => a[1] > b[1] ? 1 : a[1] < b[1] ? -1 : 0);
}

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

console.log(updateInventory(curInv, newInv));
/*
[[88, "Bowling Ball"], [2, "Dirty Sock"], [3, "Hair Pin"], [3, "Half-Eaten Apple"], [5, "Microphone"], [7, "Toothpaste"]]
*/
console.log(updateInventory([[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]], []));
/*
[[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]]
*/
console.log(updateInventory([], [[2, "Hair Pin"], [3, "Half-Eaten Apple"], [67, "Bowling Ball"], [7, "Toothpaste"]]));
/*
[[67, "Bowling Ball"], [2, "Hair Pin"], [3, "Half-Eaten Apple"], [7, "Toothpaste"]]
*/
console.log(updateInventory([[0, "Bowling Ball"], [0, "Dirty Sock"], [0, "Hair Pin"], [0, "Microphone"]], [[1, "Hair Pin"], [1, "Half-Eaten Apple"], [1, "Bowling Ball"], [1, "Toothpaste"]]));
/*
[[1, "Bowling Ball"], [0, "Dirty Sock"], [1, "Hair Pin"], [1, "Half-Eaten Apple"], [0, "Microphone"], [1, "Toothpaste"]]
*/
```
🔐 solution1
```javascript
function updateInventory(currentInventory, newInventory) {
  // Check each item of new inventory
  for (let newItem of newInventory) {
    let found = false;
    // Check against current inventory
    for (let oldItem of currentInventory) {
      // Update value if new item is found
      if (newItem[1] === oldItem[1]) {
        oldItem[0] += newItem[0];
        found = true;
        break;
      }
    }
    // Otherwise add item to the old inventory
    if (!found) currentInventory.push([...newItem]);
  }
  // Alphabetize and put into original array format
  return currentInventory
    .sort((a, b) => {
        if (a[1] < b[1]) return -1;
        if (a[1] > b[1]) return 1;
        return 0;
    });
}
```

## No Repeats Please

Return the number of total permutations of the provided string that don't have repeated consecutive letters. Assume that all characters in the provided string are each unique.

For example, `aab` should return 2 because it has 6 total permutations (`aab`, `aab`, `aba`, `aba`, `baa`, `baa`), but only 2 of them (`aba` and `aba`) don't have the same letter (in this case a) repeating.  


📝
```javascript
```
🔐 solution1
```javascript
```

## Pairwise
Given an array `arr`, find element pairs whose sum equal the second argument `arg` and return the sum of their indices.

You may use multiple pairs that have the same numeric elements but different indices. Each pair should use the lowest possible available indices. Once an element has been used it cannot be reused to pair with another element. For instance, `pairwise([1, 1, 2], 3)` creates a pair `[2, 1]` using the 1 at index 0 rather than the 1 at index 1, because 0+2 < 1+2.

For example `pairwise([7, 9, 11, 13, 15], 20)` returns `6`. The pairs that sum to 20 are `[7, 13]` and `[9, 11]`. We can then write out the array with their indices and values.  

Below we'll take their corresponding indices and add them.

7 + 13 = 20 → Indices 0 + 3 = 3  
9 + 11 = 20 → Indices 1 + 2 = 3  
3 + 3 = 6 → Return 6

📝  
- 배열을 순회하면서 [item, index]를 요소 값으로 가지는 새로운 배열 itemIndex을 만든다.
- 중첩 반복문을 통해 현재 요소 숫자 값을 기준으로 그 다음의 index를 돌면서 pair넘버를 찾는다.
- 값이 있다면, sum에 현재 요소(i)의 index값인 [item, index]의 index 값과 j로 찾은 요소의 index값을 더한다.
- 그리고 같은 값을 또 발견하는 일이 없게 sum에 사용된 요소들은 null로 바꾼다.
- 만약 현재 반복문의 요소가 null이라면 다음 반복문으로 넘어가고,
- 중첩 반복문에서 이 과정을 마치면 break를 통해 다음의 j를 찾지 않고 다음 i를 찾는다. 
```javascript
function pairwise(arr, arg) {
  let itemIndex = [];
  let sum = 0;

  arr.forEach((item, index) => {
    itemIndex.push([item, index]);
  })

  for(let i = 0; i < itemIndex.length; i++) {
    if(itemIndex[i] == null) continue;

    const pairNum = arg - itemIndex[i][0];

    for(let j = i + 1; j < itemIndex.length; j++) {
      if(itemIndex[j] == null) continue;
      if(pairNum == itemIndex[j][0]) {
        sum += itemIndex[i][1] + itemIndex[j][1];
        itemIndex.splice(itemIndex[i][1], 1, null);
        itemIndex.splice(itemIndex[j][1], 1, null);
        break;
      }
    }
    console.log('after break')
  }
  return sum;
}

console.log(pairwise([1,4,2,3,0,5], 7)); // 11
console.log(pairwise([1, 3, 2, 4], 4)); // 1
console.log(pairwise([1, 1, 1], 2)); // 1
console.log(pairwise([0, 0, 0, 0, 1, 1], 1)); // 10
console.log(pairwise([], 100)); // 0
```
🔐 solution1
```javascript
function pairwise(arr, arg) {
  let pairIndices = [];
  // Check every pair
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      // Exclude pairs that contain previously paired elements
      if (arr[i] + arr[j] == arg
          && !pairIndices.includes(i)
          && !pairIndices.includes(j)) {
        pairIndices.push(i, j);
        break;
      }
    }
  }

  return pairIndices.reduce((sum, curr, index) => sum + curr, 0);
}

pairwise([1,4,2,3,0,5], 7);
```

## Implement Bubble Sort

This is the first of several challenges on sorting algorithms. Given an array of unsorted items, we want to be able to return a sorted array. We will see several different methods to do this and learn some tradeoffs between these different approaches. While most modern languages have built-in sorting methods for operations like this, it is still important to understand some of the common basic approaches and learn how they can be implemented.

Here we will see bubble sort. The bubble sort method starts at the beginning of an unsorted array and 'bubbles up' unsorted values towards the end, iterating through the array until it is completely sorted. It does this by comparing adjacent items and swapping them if they are out of order. The method continues looping through the array until no swaps occur at which point the array is sorted.

This method requires multiple iterations through the array and for average and worst cases has quadratic time complexity. While simple, it is usually impractical in most situations.

Instructions: Write a function `bubbleSort` which takes an array of integers as input and returns an array of these integers in sorted order from least to greatest.
📝
```javascript
```
🔐 solution1
```javascript
```

##
📝
```javascript
```
🔐 solution1
```javascript
```

##
📝
```javascript
```
🔐 solution1
```javascript
```

##
📝
```javascript
```
🔐 solution1
```javascript
```

##
📝
```javascript
```
🔐 solution1
```javascript
```

##
📝
```javascript
```
🔐 solution1
```javascript
```

##
📝
```javascript
```
🔐 solution1
```javascript
```
