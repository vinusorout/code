Personal Learning

# Arrays

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


## You are given an array of positive integers where each integer represents the height of a vertical line on a chart. Find two lines which together with the x-axis forms a container that would hold the greatest amount of water. return the area of water it would hold.

![image](https://user-images.githubusercontent.com/27411868/124641595-84e0d380-deac-11eb-9fc1-9f4376830f4f.png)

* Edge condition 1: [] empty array >> return 0
* Edge condition 2: [7] 1 length array >> return 0

### Working Solution T = O(n ^ 2) and S = O(1)
```js
funstion getMaxArea(numbers) {
  let maxArea = 0;
  for(let i = 0; i < numbers.length; i++) {
    for(let j = i+1; j < numbers.length; j++) {
      const height = Math.min(numbers[i], number[j]);
      const width = j - i;
      const area = height * width;
      if(area > maxArea) {
        maxArea = area;
      }
    }
  }
  return maxArea;
}
```

### Optimal Solution (using technique two shifting pointers) O(n)

### NOTE: For two pointer we need to find a condition so that we can move one at a point.

```js
// starts the two pointers from both side of array
funstion getMaxArea(numbers) {
  let pointerOne = 0;
  let pointerTwo = numbers.length - 1;
  let maxArea = 0;
  while (pointerOne < pointerTwo) {
    const height = Math.min(numbers[pointerOne], numbers[pointerTwo]);
    const width = pointerTwo - pointerOne;
    const area = height * width;
    maxArea = Math.max(maxArea, area);
    if(numbers[pointerOne] =< numbers[pointerTwo]) {
      pointerOne++;
    } else {
      pointerTwo--;
    }
  }
  return maxArea;
}
```


## Trapping Rainwater, Given an array of integers representing an elevation map where the width of each bar is 1, return how much rainwater can be trapped.
![image](https://user-images.githubusercontent.com/27411868/124649006-8f539b00-deb5-11eb-8ece-2237fb81a407.png)

Answer is 8 in the above figure.

* Edge Condition 1: [] empty array, return 0
* Edge Condition 2: [1] one length, return 0
* Edge Condition 3: [4,4,3], return 0 because the graph is full from left side, and open in right side 

* We have to find MAX values to the left Max value sto the right and inbetween these two the blocks will simpally minus from area.

### Working Solution: O(n ^ 2)
```js
function getTrappedWater(numbers) {
  let totalWater = 0;
  for(let currentNumber = 0; currentNumber< numbers.length; currentNumber++) {
    let leftOfcurrentNumber = currentNumber;
    let rightOfcurrentNumber = currentNumber;
    let maxLeft = 0;
    let maxRight = 0;
    while(leftOfcurrentNumber >= 0) {
      maxLeft = Math.max(maxLeft, numbers[leftOfcurrentNumber]);
      leftOfcurrentNumber--;
    }
    while(rightOfcurrentNumber < numbers.length) {
      maxRight = Math.max(maxRight, numbers[rightOfcurrentNumber]);
      rightOfcurrentNumber++;
    }
    const currentWater = Math.min(leftOfcurrentNumber, rightOfcurrentNumber) - numbers[currentNumber];
    if(currentWater > 0) {
      totalWater += currentWater;
    }
  }
  return totalWater;
}
```

### Optimal Solution (two pointer shift technique) O(n):
```js
const getTrappedRainwater = function(heights) {

  let left = 0, right = heights.length - 1, totalWater = 0, maxLeft = 0, maxRight = 0;
  
  while(left < right) {
    if(heights[left] <= heights[right]) {
      if(heights[left] >= maxLeft) { 
        maxLeft = heights[left]
      } else { 
        totalWater += maxLeft - heights[left];
      }
      left++;
    } else {
      if(heights[right] >= maxRight) {
          maxRight = heights[right];
      } else {
          totalWater += maxRight - heights[right];
      }
        
      right--;
    }
  }

  return totalWater;
}
```


# Strings

## Given two strings S and T, return if they equal when both are typed out. Any '#' that appears in the string counts as backspace.

* "ab#c" --> typed out string will be "ac"
* inputs S: "ab#c" T: "az#c"    --> output "ac"
* Constriant 1: What happens when two #'s appear beside each other? --> Delete the two values before the first #.  "cab###" --> "c"
* "a###b" --> "b"    (two # wont delete anything).
* Are two empty strings equal to each other? --> Yes
* Does case sensitivity matter? --> Yes

### Working solution:
```js
const buildString =  function(string) {   // O(n)
  const finalArray = [];
  for(let i =0; i < string.length; i++) {
    if(string[i] !== '#' ) {
      finalArray.pus(string[i]);
    } else {
      finalArray.pop();
    }
  }
  return finalArray;
}

cont finalSol = function(S, T) {           // So O(2a+b) or O (a+2b) --> ignore constants final O(a+b),   Space is also S(a+b)
  const finalS = buildString(S); // O(a) size of S
  const finalT = buildString(T); // O(b) size of S
  if(finalS.length !== finalT.length) {
    return false;
  } else {
    for(let i = 0; i < finalS.length; i++) {  // either O(a) or O(b)
      if(finalS[i] !== finalT[i]) {
        return false;
      }
    }
  }
  return true;
}
```

### Optimal solution: reduced the space complexity.
* Hint: try to utilize the original strings
* Hint 2: Use the 2 pointer technique
* Hint 3: Start from the end of the strings

```js
// for single '#' move pointer -2, but for next concurrent move -2 + -1 and so on....


```











LeetCode
