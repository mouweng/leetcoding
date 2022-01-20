# @TIGER-H

## 2021-12-30

### 846

```javascript
/**
 * @param {number[]} hand
 * @param {number} groupSize
 * @return {boolean}
 */
var isNStraightHand = function(hand, groupSize) {
    const cardList = {}
    hand.forEach(card => cardList[card] ? cardList[card]++ : cardList[card] = 1)
    const cards = Object.keys(cardList).sort((a, b) => a - b)
    
    for(let i=0; i<cards.length; i++) {
        const card = cards[i] - 0
        const c = cardList[card]
        if(c === 0) continue
        
        for(let j=card; j<groupSize+card; j++){
            if(cardList[j] === undefined) return false
            
            cardList[j] -= c
            
            if(cardList[j] < 0) return false
        }
    }
    return true
};
```

## 2021-12-31

### 507

直接遍历 耗时 3440 ms 

```javascript
/**
 * @param {number} num
 * @return {boolean}
 */
var checkPerfectNumber = function(num) {
  let sum = 1;  
  if(num===1) return false
  for(let i=2; i*i < num; i++) {
      if(num % i === 0) {
        sum += i;
        sum += num / i;
      }
    // if(sum > num) return false
    }
  if(sum === num) return true
  else return false
};
```

### 1185

```javascript
/**
 * @param {number} day
 * @param {number} month
 * @param {number} year
 * @return {string}
 */
const dayOfTheWeek = (day, month, year) => 
  (new Intl.DateTimeFormat('en-US', {
    weekday: 'long'
  })
  .format(new Date(year, month-1, day)))
```

## 2022-01-05

### 1576


```javascript
/**
 * @param {string} s
 * @return {string}
 */

var modifyString = function(s) {
    let str = s.split("")
    
    for(let i=0; i<str.length; i++){
        let alphabet =  ['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z']
        if(str[i] === "?"){
           str[i-1] ?
              alphabet = alphabet.filter(word => word !== str[i-1]) 
          :
           str[i+1] ?
              alphabet = alphabet.filter(word => word !== str[i+1]) 
          :
          str[i] = alphabet[0]
        }
    }
    return str.join("")
};
```

### 146

提交了几次过了lmao

```javascript
/**
 * @param {number} capacity
 */

// doubly linkedlist and hashmap? 
var LRUCache = function(capacity) {
    this.capacity = capacity
    this.map = new Map()
    
    this.head = {}
    this.tail = {}
    
    this.head.next = this.tail
    this.tail.prev = this.head
};

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function(key) {
   if(this.map.has(key)){
       // 4 ptrs to change
       let node = this.map.get(key)
       node.prev.next = node.next
       node.next.prev = node.prev
       
       this.tail.prev.next = node
       node.next = this.tail
       node.prev = this.tail.prev
       this.tail.prev = node
       
       return node.value
   } else {
       return -1
   }
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function(key, value) {
    // exist
    if(this.get(key) !== -1){
        this.tail.prev.value = value
    } else {
        if(this.map.size === this.capacity){
            this.map.delete(this.head.next.key)
            this.head.next = this.head.next.next
            this.head.next.prev = this.head
        }
        
        const newNode = {
            key, value
        }
        
        this.map.set(key, newNode)
        this.tail.prev.next = newNode
        newNode.prev = this.tail.prev
        newNode.next = this.tail
        this.tail.prev = newNode  
    }
};

/** 
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
 ```

 ## 2022-01-06

 ### 71 

 using a stack

 ```javascript
 /**
 * @param {string} path
 * @return {string}
 */
var simplifyPath = function(path) {
    const canonicalPath = path.split('/')
    let stack = []
    for(let i=0; i<canonicalPath.length; i++) {
      if(canonicalPath[i] === '.' || canonicalPath[i] === ""){
        // same level, do nothing
      } else if(canonicalPath[i] === '..'){
        // level up
        if(stack.length !== 0) stack.pop()
      } else {
        stack.push(canonicalPath[i])
      }
    }
  return "/" + stack.join('/')
};
 ```

 ### 3

sliding window pattern

> in this case, we can use a fixed-size array (arr[25])

 ```javascript
 /**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
  // sliding window pattern
  let windowStart = 0,
      maxLength = 0,
      existed = {}
  
  for(let windowEnd=0; windowEnd < s.length; windowEnd++){
    const cur = s[windowEnd]
    if(cur in existed){
      // exists in existed object
      windowStart = Math.max(windowStart, existed[cur] + 1) 
    } 
    // push 
    existed[cur] = windowEnd
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1) 
  }
  
  return maxLength
};
 ```

Some extends:

### Problem statement

Given a string, find the length of the *longest substring* in it *with no more than `k` distinct characters*.
Leetcode premium 里的题目...

```javascript
  const longest_substring_with_k_distinct = (str, k) =>{
    // 思路应该是类似的 多记录一些内容
    let windowStart = 0,
    maxLength = 0,
    charFreq = {}

    for(let windowEnd=0; windowEnd < str.length; windowEnd++){
      const rightChar = str[windowEnd]
      (rightChar in charFreq) ? charFreq[rightChar]++ : charFreq[rightChar] = 1

      // shrink the window, until we are left with k distinct chars in charFreq
      while(Object.keys(charFreq).length > k){
        const leftChar = str[windowStart]
        charFreq[leftChar] -= 1
        if(charFreq[leftChar] === 0){
          delete charFreq[leftChar]
        }
        windowStart += 1
      }

      maxLength = Math.max(maxLength, windowEnd - windowStart + 1)
     }
    return maxLength
  }
```


## 2022-01-07

### 1614

搞半天自己就是VPS...

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var maxDepth = function(s) {  
  let stack =[],
      depth = 0
  for(let i=0; i<s.length; i++){
    if(s[i] === "("){
      stack.push(s[i])
      if(stack.length > depth) depth = stack.length
    } else if(s[i] === ")"){
      stack.pop()
    }
  }
  return depth
};
```

### 215

where the f**k am I?

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
const findKthLargest = (nums, k) => nums.sort((a,b) => b - a)[k-1]
```

## 2022-01-12

### 334 

find min, find 2nd min, ...

```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var increasingTriplet = function(nums) {
  let one = Infinity, two = Infinity
  let ret = false
  nums.forEach(num => {
    if(num <= one){
      one = num
    } else if(num <= two){
      two = num
    } else {
      ret = true
    }
  })
  return ret
};
```

## 2022-01-16

### 382

```javascript
  // get length
  /**
  * Definition for singly-linked list.
  * function ListNode(val, next) {
  *     this.val = (val===undefined ? 0 : val)
  *     this.next = (next===undefined ? null : next)
  * }
  */
  /**
  * @param {ListNode} head
  */
  var Solution = function(head) { 
    this.list = [];
    
    let cur = head;
    while(cur){
      this.list.push(cur.val);
      cur = cur.next;
    }
    
    this.length = this.list.length;
  };

  /**
  * @return {number}
  */
  Solution.prototype.getRandom = function() {
    const rdnum = Math.floor(Math.random() * this.length);
    return this.list[rdnum]
  };

  /** 
  * Your Solution object will be instantiated and called as such:
  * var obj = new Solution(head)
  * var param_1 = obj.getRandom()
  */
```

```javascript
// What if the linked list is extremely large and its length is unknown to you?
// https://en.wikipedia.org/wiki/Reservoir_sampling
// 慢爆
  /**
  * Definition for singly-linked list.
  * function ListNode(val, next) {
  *     this.val = (val===undefined ? 0 : val)
  *     this.next = (next===undefined ? null : next)
  * }
  */
  /**
  * @param {ListNode} head
  */
  var Solution = function(head) {
    // RESERVOIR SAMPLING
    this.head = head;
  };

  /**
  * @return {number}
  */
  Solution.prototype.getRandom = function() {
    let cur = this.head;
    let range = 1, result = 0;
    while(cur){
      if(Math.random() < 1/range){
        result = cur.val;
      }
      range++;
      cur = cur.next;
    }
    return result;
  };

  /** 
  * Your Solution object will be instantiated and called as such:
  * var obj = new Solution(head)
  * var param_1 = obj.getRandom()
  */
```

## 2022-01-18

### 539

```javascript
/**
 * @param {string[]} timePoints
 * @return {number}
 */
const reducer = (prev, cur) => {
  let [prev_i, cur_i] = [prev, cur].map(i => parseInt(i));
  if(prev_i === 0){
    prev_i = 24;
  }
  return prev_i*60 + cur_i;
}

var findMinDifference = function(timePoints) {
  const minutes = timePoints
  .map(time => time.split(':').reduce(reducer))
  .sort((a,b) => a-b)
  const diff = minutes
  .map((min, idx, arr) => idx === 0 ? min : min - arr[idx-1])
  .slice(1)

  return Math.min(...diff, 1440 + minutes[0] - minutes.pop());
};
```

### 88

```javascript
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */

// nums1.length = m + n
var merge = function(nums1, m, nums2, n) {
  for(let i=m-1, j=n-1, k=n+m-1; j>=0; ){
    if(nums1[i] > nums2[j]){
      nums1[k] = nums1[i];
      i--;
    } else {
      nums1[k] = nums2[j];
      j--;
    }
    k--;
  }
};
```

## 2022-01-19

### 219

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {boolean}
 */
var containsNearbyDuplicate = function(nums, k) {
  const map = new Map();
  for(let i=0; i<nums.length; i++){
    if(map.has(nums[i]) && i-map.get(nums[i]) <= k){
       return true;
    }
    map.set(nums[i], i);
  }
  return false;
};
```