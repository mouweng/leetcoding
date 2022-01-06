# @TIGER-H

## 2021-12-30

### 846

感觉很蠢

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