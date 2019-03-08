# Question 4: Candies

## Part 1 Recursive Implementation
```javascript
// Complete the candies function below.
function candies(n, arr) {
    let candy = new Array(n).fill(0)
    for (var i in arr) {
        let idx = Number(i);
        //console.log(arr);
        //console.log("left");
        let l = left(idx - 1, arr, candy);
        //console.log("right");
        let r = right(idx + 1, arr, candy);
        //console.log("Candy: ", arr[i], " @ ", i, ": left ", l, " right ", r);
        candy[Number(i)] = Math.max(l, r);
    }
    return candy.reduce(function (acc, cur) {
        return acc + cur;
    });
}

function left(i, arr, candy) {
    //console.log(i);
    if (i < 0 || arr[i] >= arr[i + 1]) {
        return 1
    } else if (candy[i] != 0){
        if (arr[i] < arr[i + 1]) {
            return candy[i] + 1;
        } else {
            return candy[i];
        }
    } else {
        return left((i - 1), arr, candy) + 1;
    }
    
}

function right(i, arr, candy) {
    //console.log(i);
    if (i > arr.length - 1 || arr[i] >= arr[i - 1]) {
        return 1
    } else if (candy[i] != 0) {
        if (arr[i] < arr[i - 1]) {
            return candy[i] + 1;
        } else {
            return candy[i];
        }
    } else {
        return right((i + 1), arr, candy) + 1
    }
}

```
## Part 2 Memoization Table

Score| 4 | 6 | 4 | 5 | 6 | 2 |
---|---|---|---|---|---|---
1 | 1 | 0 | 1 | 0 | 0 | 1
2 | 1 | 2 | 1 | 2 | 0 | 1
3 | 1 | 2 | 1 | 2 | 3 | 1

## Part 3 Explanation

The X axis is the students score and the Y axis represent the amount of candies. The evaluation for each cell on a row is if each neighbor is greater than itself or both of its neighbors candies have been set. If the score is less than both their neighbors then the number of candies is equal to 1. If the both neighbors candies have been set then you take the max candies plus 1. You repeat this until all students have candies set.

## Part 4 Dynamic Solution
```javascript
function candies(n, arr) {
    let candy = new Array(n).fill(0)
    for (var i in arr) {
        let idx = Number(i);
        let l = 1;
        var j = idx - 1;
        while (j >= 0 && arr[j] < arr[j + 1]) {
            if (candy[j] != 0) {
                if (arr[j] < arr[j + 1]) {
                    l = candy[j] + 1;
                    break;
                } else {
                    l = candy[j];
                }
            } else {
                l += 1;
            }
            j--;
        }
        let r = 1;
        j = idx + 1;
        while (j <= arr.length - 1 && arr[j] < arr[j - 1]) {
            if (candy[j] != 0) {
                if (arr[j] < arr[j - 1]) {
                    r = candy[j] + 1;
                    break;
                } else {
                    r = candy[j];
                }
            } else {
                r += 1;
            }
            j++;
        }
        console.log("Candy: ", arr[i], " @ ", i, ": left ", l, " right ", r);
        candy[Number(i)] = Math.max(l, r);
    }
    console.log({ arr, candy });
    return candy.reduce(function (acc, cur) {
        return acc + cur;
    });
}
```
