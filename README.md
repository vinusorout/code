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

### Optimal solution: reduced the space complexity. T O(a+b) S: O(1)
* Hint: try to utilize the original strings
* Hint 2: Use the 2 pointer technique
* Hint 3: Start from the end of the strings

```js
// for single '#' move pointer -2, but for next concurrent move -2 + -1 and so on....
const finalSol = function(S, T) {
  let i = S.length - 1, j = T.length - 1;
  while(i >= 0 || j >= 0) {
    if(S[i] === '#' || T[j] === '#') {
      if(S[i] === '#') {
        let backCount = 2;
        while(backCount > 0){
          i--;
          backCount--;
          if(S[i] === '#') {
            backCount = backCount + 2;
          }
        }
      }
      if(T[j] === '#') {
        let backCount = 2;
        while(backCount > 0){
          j--;
          backCount--;
          if(T[j] === '#') {
            backCount = backCount + 2;
          }
        }
      }
    } else {
      if(S[i] !== T[j]) {
        return false;
      } else {
        i--;
        j--;
      }
    }
  }
  return true;
}

```

## Given a string, find the length of the longest sub string without repeating characters.

* Constraints 1: Is the substring contiguous: Yes, look for a substring not subsequence
* Contigous means, Characters that show up are sequential and do not have any breaks.
<img width="545" alt="image" src="https://user-images.githubusercontent.com/27411868/157099003-fa642651-117d-41f2-995c-4396487cd8f3.png">

* Substring: "abcbbd" --> "abc"
* SubSequence: "abcbbd" --> "abcd"


* test case "abcbda" --> 4 for("cbda")

### Working solution T = O(n ^ 2), S = O(n)
```js
const getlengthOfLongestSubString = function(s) {
  if(s.length =< 1) return s.length;
  let longestLength = 0;
  for( let i = 0; i < s.length; i++){
    const currentChars = {}, currentLength = 0;
    for( let j = i; j < s.length; j++) {
      const currentChar = s[j];
      if(!currentChars[currentChar]){
        currentChars[currentChar] = true;
        currentLength++;
        longestLength = Math.max(currentChar, longestLength);
      } else {
        break;
      }
    }
  }
  return longestLength;
}
```


#### Sliding Window Technique
From a window over some portion of sequential data, then move that window throughout the data to capture different parts of it.

Simple Example is: Given an array of integers, find two contigious numbers that forms the greatest number.
[1, 3, 7, 9, 2, 4]
For this window will be of two integers, two pointers P1 and P2 will start from 1 and 3 respectively
Then calculate the sum of values at P1 and P2, and move on until P2 and p1 reaches at the last.


### Optimal Solution using Sliding Window: T O(N), S O(N)
```js
const getLengthOfLongestSubString = function(s) {
  if(s.length =< 1) return s.length;
  let longestLength = 0;
  const seenChars = {};  // NOTE: In javascript a new "map" is very faster than "object {}", calculations is still same but while running it will be faster.
  let i = 0;
  // for second pointer use for loop
  for(let j = 0; j < s.length; j++){
    let currentChar = s[j];
    let previousSeenCharIndex = seenChars[currentChar];
    // if alredy seen move the left pointer to (this value + 1)
    if (previousSeenCharIndex >= i){
      i = previousSeenCharIndex + 1;
    }
    seenChars[currentChar] = j; // store the latest index of current char
    longestLength = Math.max(longestLength, (j - i) + 1);
  }
  return longestLength;
}

```


## Palindromes
A string that reads the same forwards and backwards. ex "aba", "a", "race car", ""   (Remove the extra chars like spave colon etc)
```js
const isValidPalindrome = function(s) {
  s = s.replace(/[^A-Za-z0-9]/g, "").toLowerCase();
  let left = 0; right = s.length - 1;
  while(left < right) {
    if(s[left] !== s[right]) {
      return false;
    }
    left++'
    right--;
  }
  return true;
}
```

### Almost A Palindrome, removing one char makes it palindrome "abccdba" -->> remode "d" --> "abccba"
```js
var validPalindrome = function(s) {
  let start = 0;
  let end = s.length - 1;
  while (start < end) {
      if (s[start] !== s[end]) {
          return validSubPalindrome(s, start + 1, end) || validSubPalindrome(s, start, end - 1);   // only one or both will be palindrome
      }
      start++;
      end--;
  }
  return true;
};

var validSubPalindrome = function(s, start, end) {
  while (start < end) {
      if (s[start] !== s[end]) {
          return false;
      }
      start++;
      end--;
  }
  return true;
};
```


# Linked List

## Reverse a given linked list. O(n) , S(1)
```js
  const revreseLinkedList = function(head) {
    let currentNode = head;
    let previousNode = null;
    while(currentNode) {
      let next = currentNode.next;
      currentNode.next = previousNode;
      previousNode = currentNode;
      currentNode = next;
    }
    return previousNode;
  }
```

## Given a linked list and numbers m and n, return it back with only position m to n in reverse.

Let sya linked list is  1 --> 2 --> --> 3 --> 4 --> 5 --> 6 --> null, and m =2 and n = 4

return 1 --> 4 --> 3 --> 2--> 5 --> 6 --> null

```js
const revreseLinkedList = function(head, m, n) {
    let currentPosition = 1;
    let currentNode = head;
    let strt = head;
    
    // find the start node first, from where we need to start reverse
    while(currentPosition < m) {
      start = currentNode;
      currentNode = currentNode.next;
      currentPosition++;
    }
    // from above ex start will be = 1 and current Node = 2
    
    let newList = null, tail = currentNode; // tail value is start of the revresal. from above it will be 2.
    
    while(currentPosition >= m && currentPosition =< n) {
      let next = currentNode.next;
      currentNode.next = newList;
      newList = currentNode;
      currentNode = next;
      currentPosition++;
    }
    // after this while currentNode will be 5, at the end of n
    
    // now link the list
    start.next = newList;    // from above ex 1 will be linked to 4
    tail.next = currentNode; // 2 will be linked to 5
    
    return m > 1 ? head : newList;
  }
```

## Merge Multi-Level Doubly linked list
Statement: Given a doubly linked list, list nodes also have child property that can point to a separate doubly linked list. These child lists can also have one or more child doubly linked lists of their own, and son on. Return the list as a single level flattened doubly linked list.
```js
// class representing linked list of above statement
class ListNode {
  value: any;
  prev: ListNode;
  next: ListNode;
  child: ListNode; // can be null of a refrence of another doubly linked list
}
```

![image](https://user-images.githubusercontent.com/27411868/125462433-9db1de02-1453-473a-8f58-9bcf3201986c.png)

After Solution:
![image](https://user-images.githubusercontent.com/27411868/125462473-2e071836-031e-479e-b2c5-6e52411a670c.png)

```js
var flatten = function (head) {
  if (!head) return head;

  let currentNode = head;
  while (currentNode !== null) {
    if (currentNode.child === null) {
      currentNode = currentNode.next;
    } else {
      let tail = currentNode.child;
      while (tail.next !== null) {
        tail = tail.next;
      }

      tail.next = currentNode.next;
      if (tail.next !== null) {
        tail.next.prev = tail;
      }

      currentNode.next = currentNode.child;
      currentNode.next.prev = currentNode;
      currentNode.child = null;
    }
  }

  return head;
};
```

## Linked List Cycle Detection:
```js
// T:O(n) S:O(n)
const findCycle = function(head) {
  let currentNode = head;
  const seenNodes = new Set();
  while(!seenNodes.has(currentNode)){
    if(currentNode.next === null) {
      return false;
    }
    seenNodes.add(currentNode);
    currentNode = currentNode.next;
  }
  return currentNode;
}
```

```js
// O(n), S(1)
// Using Floyd's Algo (Tortoise and Hare Algorithm)

const findCycle = function(head) {
  let hare = head, tortoise = head; // hare will move +2 and tortoise will move +1
  // end the cycle if hare and tortoise meet or hare reach to null(tail)
  while(true) {
    hare = hare.next;
    tortoise = tortoise.next;
    if(hare === null || hare.next === null) {
      return false;
    } else {
      hare = hare.next;
    }
    if(tortoise === hare) break;
  }
  
  // find the node
  let p1 = head; p2 = tortoise; // assign hare or tortoise as both are same here
  while( p1 !== p2){
    p1 = p1.next;
    p2 = p2.next;
  }
  
  return p1; // return either p1 or p2
}
```


# Stacks

## Given a string containing only parentheses, determine if it is valid. The string is valid if all parenthese close.

* "{([])}" --> Valid
* "{([)]}" --> Invalid

```js
const string = "{()[]}"

const parens = {
    '(': ')',
    '{' : '}',
    '[': ']'
}

var isValid = function(s) {
  if(s.length === 0) return true;
  
  const stack = [];
  
  for(let i = 0; i < s.length; i++)  {
    if(parens[s[i]]) {
      stack.push(s[i]);
    } else {
      const leftBracket = stack.pop();
      const correctBracket = parens[leftBracket];
      if(s[i] !== correctBracket) return false
    }
  }
  
  return stack.length === 0;
};

console.log(isValid(string));
```

## Given a string only containing round brackets, and lowercase characters, remove the least amount of brackets so the string is valid. A string is considered valied if it is empty or if there are brackets, they all close.

```js
const string1 = "(ab(cd)"

const minRemoveToMakeValid = function(str) {
    const res = str.split('');
    const stack = [];
    
    for (let i = 0; i < res.length; i++) {
        if (res[i] === '(') {
            stack.push(i);
        } else if (res[i] === ')' && stack.length) {
            stack.pop();
        } else if (res[i] === ')') {
            res[i] = ''
        }
    }
    
    while (stack.length) {
        const curIdx = stack.pop();
        res[curIdx] = '';
    }
    
    return res.join('');
};

console.log(minRemoveToMakeValid(string1))
```




# Queues

## Implement the class queue using stacks, the queue method you need to impelement are enqueue, dequeue, peek and empty.

```js
class QueueWithStacks {
  constructor() {
    this.in = [];
    this.out = [];
  }

  enqueue(val) {
    this.in.push(val);
  }

  dequeue() {
    if (this.out.length === 0) {
      while(this.in.length > 0) {
        this.out.push(this.in.pop());
      }
    }
    
    return this.out.pop();
  }

  peek() {
    if (this.out.length === 0) {
        while(this.in.length > 0) {
            this.out.push(this.in.pop());
        }
    }
    
    return this.out[this.out.length - 1];
  }

  empty() {
    return this.in.length === 0 && this.out.length === 0;
  }
};
```

# Recursion

## Given an unsorted array, return the kth largest element in sorted order, not the kth distinct element.

* Given [2,3,1,2,2,4] --> find the 4th --> sorted [1,2,2,2,3,4] andswer is 2 not 1 as we have to consider duplicate values.
### Using Quick Sort:

```js
const array = [5,3,1,6,4,2];
const kToFind = 4;

const swap = function (array, i, j) {
  const temp = array[i];
  array[i] = array[j];
  array[j] = temp;
};

const getPartition = function (nums, left, right) { //[5,3,1,6,4,2], 0, 5 --> second call [1,2,5,6,4,3], 2, 5  --> [1,2,3,6,4,5], 3, 5
  const pivotElement = nums[right]; // 2 --> 3 --> 5
  let partitionIdx = left; // 0 --> 2 --> 3

  for (let j = left; j < right; j++) {
    if (nums[j] <= pivotElement) { // when j = 2, for secodn --> no entry in if --> when j = 4 swap 3 and 4 [1,2,3,4,6,5]
      swap(nums, partitionIdx, j); // [1,3,5,6,4,2]
      partitionIdx++; // 1 --> --> on third 4
    }
  }
  swap(nums, partitionIdx, right) // [1,2,5,6,4,3] --> [1,2,3,6,4,5] --> [1,2,3,4,5,6]

  return partitionIdx; // 1 --> 2 --> 4
};

const quickSort = function (nums, left, right) {
  if(left < right) {
    const partitionIndex = getPartition(nums, left, right); // 1 --> 2

    quickSort(nums, left, partitionIndex - 1); // left 0, right 0 will simplly return [1,2,5,6,4,3] --> again return as left left = 2 right = 1 [1,2,3,6,4,5]
    quickSort(nums, partitionIndex + 1, right); // left 2, right 5 --> left 3 right 5
  }
};

var findKthLargest = function (nums, k) {
  const indexToFind = nums.length - k;
  quickSort(nums, 0, nums.length - 1);
  return nums[indexToFind]
};

console.log(findKthLargest(array, kToFind))
```

## Given an array of integers sorted in ascending order, return the starting and ending index of a given target value in an array, i.e. [x, y]. With solution O(logN) time.

* Input [1,3,3,5,5,5,8,9] target: 5 --> return [3, 5]
* Binary search involved
* Find the first mid point where we found the target and then checl left and right of that mid point if it is same target.

```ts
const array = [1,3,3,5,5,5,8,9];
const targetToFind = 5;

const binarySearch = (nums, left, right, target) => {
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    const foundVal = nums[mid];
    if (foundVal === target) {
      return mid;
    } else if (foundVal < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return -1;
};

const searchRange = function (nums, target) {
  if (nums.length < 1) return [-1, -1];
  const firstPos = binarySearch(nums, 0, nums.length - 1, target);

  if (firstPos === -1) return [-1, -1];

  let endPos = firstPos,
    startPos = firstPos,
    temp1,
    temp2;

  while (startPos !== -1) {
    temp1 = startPos;
    startPos = binarySearch(nums, 0, startPos - 1, target);
  }
  startPos = temp1;

  while (endPos !== -1) {
    temp2 = endPos;
    endPos = binarySearch(nums, endPos + 1, nums.length - 1, target);
  }
  endPos = temp2;

  return [startPos, endPos];
};

console.log(searchRange(array, targetToFind))
```






LeetCode
