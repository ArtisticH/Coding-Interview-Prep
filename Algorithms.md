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
