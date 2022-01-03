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
