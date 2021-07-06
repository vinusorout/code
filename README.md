LeetCode

## Given an array of integres, return the indices of the two numbers that add up to a given target.

We will get two argumates:
* first array of integers [ 1,3,7,92 ] and second a target number 11
* Now we need to find if there are any two numbers in array that add up to target number 11. And then return indices(array index or position starting from 0) of these numbers. 

### Step 1 verify the constraints:
* Are all the numbers positive or can there be nagatives?  >> All numbers are positive
* Are there duplicate numbers in the array? >> No
* Will there always be a solution available? >> No, there may not always be a solution
* What do we return if there's no solution? >> return null
* Can there be multiple pairs that add up to the target? No, only 1

### Step 2: Write out some test cases.
* Best case [1,3,7,9,2] , target = 11, return [3, 4]
* No Solution, [1,3,7,9,2] , target = 25 , return null
* Edge condition 1: input blank array [], target =1, return null
* Edge condition 2: input [5] , target = 5, return null (because no two numbers) 

### Step 3: figure out a solution without code:
Find out a working solution, not optimal one

### Step 4: write out our solution in code:
```js
function findTwoSum(numbers, target) {
  for(let i = 0; i < numbers.length; i++) {
    const firstNumber = numbers[i];
    for (let j = i+1; j < numbers.length; j++) {
      if(firstNumber + numbers[j] === target) {
        return [i, j];
      }
    }
  }
  return null;
}
```

### Step 5: Analyzing Time and Space Complexity:
* Our solution is of O(n ^ 2) and space complexity is O(1)

### Step 6: Optimize the solution:
* remove nested loop so we can get O(n) complexity
* Use has map(dictionary in C# objects in JS) as searching in has map for a key ha time complexity of O(1)
```js
function findTwoSum(numbers, target) {
  const numbersMap = {};
  for(let i = 0; i < numbers.length; i++) {
    const currentMapValue = numbersMap[numbers[i]];
    if(currentMapValue >= 0) {
      return [currentMapValue, i];
    } else {
      const numberToFind = target - numbers[i];
      numbersMap[numberToFind] = i;
    }
  }
  return null;
}
```
* T = O(n) also S = O(n)









