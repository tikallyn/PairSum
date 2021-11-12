# Pair Sum

---

## Interviewer Prompt

Given an array of numbers and a target number (a "sum"), determine if any 2 numbers in the array add up to the sum.
Return true if any 2 different numbers within the array add up to sum.
Return false if no 2 numbers in the array add up to sum.

---

## Example Output

```javascript
pairSum([5, 2, 6, 9, 3], 15);    //true
pairSum([5, 2, 6, 9, 3], 13);    //false
pairSum([5], 10);    //false
```

---

## Interviewer Guide

Candidates may approach this problem in a few ways. If they can reach a brute force solution quickly, let them! You can always work on the optimization after.

---

### RE

At this point the interviewer should be asking you questions to clarify the problem statement. If they are not, prompt them: _"Do you have any questions before we get started?"_ Some questions you may get.
  - _Is the array is sorted?_ No, it does not need to be.
  - _Will the array only contain positive integers?_ No, all integers are valid.
  - _What do you want to return if the array has 0 or 1 value?_ False.

---

### AC

If the interviewee is having trouble getting started, feel free to start guiding them to the more optimal approach in this case.
  - What if they knew that they could do this problem in `O(N)` time?
  - How could you store information as you iterated through the array?
  - What information could you store?

---

### TO

If your interviewee finishes, ask them:
  - Time and space complexity of their solution
  - Can they find a solution that takes O(N) time? (if they've chosen a less optimal approach)
  - What if the array were sorted? Would your approach change?

---

### Answers to Common Questions


---

## Solution and Explanation (a)

This problem may already be familiar to a lot of you! A typical naive approach may be to add each number to every other number in the array using a nested loop.

**Time**: `O(N^2)` 

**Space**: `O(1)`

---

### Solution Code

```javascript
function pairSum(arr, sum) {
  for(let i = 0; i < arr.length; i++) {
  
    // start the inner loop at the index immediately following the current outer loop index, since you'll have already seen the sums of the preceding combinations of values already
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] + arr[j] === sum) return true
    }
  }
  return false;
}
```

---

## Solution and Explanation (b)

Some candidates might optimize by sorting their arrays before _ratcheting_, or using pointers to narrow in on the value.

**Time**: `O(N * log(N))` - We can estimate the sorting method to take `O(N * log(N))` time, which trumps the `O(N)` time it would take to iterate through the array. 

**Space**: `O(1)`

---

### Solution Code

```javascript
function pairSum(arr, sum) {
  // sort the array in ascending order (least to greatest)
  const sortedArr = arr.sort((a, b) => a -b);
  
  // initialize two pointers at each end of the array
  let left = 0;
  let right = arr.length - 1;
  
  // loop through the array until the right and left pointers are at the same index
  while (left !== right) {
    const currentSum = sortedArr[left] + sortedArr[right];
    if (currentSum === sum) {
      return true;
      
    // if the current sum of the values at the two pointers are greater than the target, you know you're looking for a smaller sum
    // decrement the right pointer to decrease the value of an addend
    } else if (currentSum > sum) {
      right--;
    
    // otherwise, the sum is too small
    // increment the left pointer to increase the value of an addend
    } else {
      left++;
    }
  }
  return false;
}
```

---

## Solution and Explanation (c)

A more optimal solution for an unsorted array of integers involves using a hash table or set to store values as we see them. Because we know what the target sum is, as we iterate through the array and look at each element, we can calculate the corresponding value we need to add to the element to reach our target. More specifically, this is the difference between the target and the current element, which we can refer to as its "complement". 

If the element's complement doesn't exist in our cache/memo, we can add that element to the cache so that a number further along in the array may recognize the value as its complement and find it.

As we continue iterating through the array, if we find a number whose complement exists in our cache/memo, we know that a pair of numbers that add up to our target exist.

**Time**: `O(N)`

**Space**: `O(N)`

---

### Solution Code

```javascript
function pairSum(arr, sum) {
  const memo = {};
  
  for (let num of arr) {
    // if we've seen the complement of number before, we know that a pair exists
    if (memo[sum - num]) {
      return true;
    // otherwise, add the number to the memo;
    } else {
      memo[num] = true;
    }
  }
  
  return false;
}
```

Another using the [Set class in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set). 
```javascript
function pairSum(arr, sum) {
  // initialize new set, which is a class/data structure that only stores unique values
  const set = new Set();
  
  for (let num of arr) {
    // check if value exists in the set;
    if (set.has(sum - num)) {
      return true;
    // otherwise, add the number to the set;
    } else {
      set.add(num)
    }
  }
  
  return false;
}
```

---

## Summary

- The optimized solution uses memoization with an additional data structure (a hash table/javascript object) in order to cache a history of values we've seen in the past so that we can access them in O(1) time as we traverse through the array. 
- In the second approach, we sorted the array before ratcheting. Since sorting can be approximated to be an `O(N * log(N))` operation, this makes pre-sorting worthwhile as it is an improvement over `O(N^2)`.
- Bonus: What if the input array were already sorted? What would be the most optimal solution there?

---
