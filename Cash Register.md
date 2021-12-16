# Cash Register

Design a cash register drawer function checkCashRegister() that accepts purchase price as the first argument (price), payment as the second argument (cash), and cash-in-drawer (cid) as the third argument.

cid is a 2D array listing available currency.

The checkCashRegister() function should always return an object with a status key and a change key.

Return {status: "INSUFFICIENT_FUNDS", change: []} if cash-in-drawer is less than the change due, or if you cannot return the exact change.

Return {status: "CLOSED", change: [...]} with cash-in-drawer as the value for the key change if it is equal to the change due.

Otherwise, return {status: "OPEN", change: [...]}, with the change due in coins and bills, sorted in highest to lowest order, as the value of the change key.

| Currency            | Unit   | Amount        |
| :-----------------: |:------:| :------------:|
| Penny               | $0.01  | (PENNY)       |
| Nickel              | $0.05  | (NICKEL)      |
| Dime                | $0.1   | (DIME)        |
| Quarter             | $0.25  | (QUARTER)     |
| Dollar	            | $1     | (ONE)         |
| Five Dollars        | $5     | (FIVE)        |
| Ten Dollars	        | $10    | (TEN)         |
| Twenty Dollars      | $20    | (TWENTY)      |
| One-hundred Dollars | $100   | (ONE HUNDRED) |

	
See below for an example of a cash-in-drawer array:

```javascript
[
  ["PENNY", 1.01],
  ["NICKEL", 2.05],
  ["DIME", 3.1],
  ["QUARTER", 4.25],
  ["ONE", 90],
  ["FIVE", 55],
  ["TEN", 20],
  ["TWENTY", 60],
  ["ONE HUNDRED", 100]
]
```

### Solution :smile:

```javascript
function checkCashRegister(price, cash, cid) {
  let currencyAmount = [["PENNY", 0.01], ["NICKEL", 0.05], ["DIME", 0.1], ["QUARTER", 0.25], ["ONE", 1], ["FIVE", 5], ["TEN", 10], ["TWENTY", 20], ["ONE HUNDRED", 100]]
  let final = {status: "", change:[]}
  let total = cid.reduce((sum, el) => sum + el[1], 0)
  let change = cash - price;
  
  if (change == total) {
    final.status = "CLOSED"
    final.change = cid
    return final
  }
  if (change > total) {
    final.status = "INSUFFICIENT_FUNDS"
    return final
  }
  for (let i = cid.length-1; i >= 0; i--) {
    let amount = 0
    while (change >= (currencyAmount[i][1]-0.001) && cid[i][1] > 0) {
      change -= currencyAmount[i][1]
      cid[i][1] -= currencyAmount[i][1]
      amount += currencyAmount[i][1]
    }
    if (amount > 0) {
      final.change.push([cid[i][0], amount])
      i = cid.length-1
    }
  }
  if (change > 0) {
    final.status = "INSUFFICIENT_FUNDS"
    final.change = []
    return final
  }
  
  final.status = "OPEN"
  return final;
}

checkCashRegister(19.5, 20, [["PENNY", 0.01], ["NICKEL", 0], ["DIME", 0], ["QUARTER", 0], ["ONE", 1], ["FIVE", 0], ["TEN", 0], ["TWENTY", 0], ["ONE HUNDRED", 0]]); 
// returns {status: "INSUFFICIENT_FUNDS", change: []}

checkCashRegister(3.26, 100, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.1], ["QUARTER", 4.25], ["ONE", 90], ["FIVE", 55], ["TEN", 20], ["TWENTY", 60], ["ONE HUNDRED", 100]]) 
// returns {status: "OPEN", change: [["TWENTY", 60], ["TEN", 20], ["FIVE", 15], ["ONE", 1], ["QUARTER", 0.5], ["DIME", 0.2], ["PENNY", 0.04]]}

checkCashRegister(19.5, 20, [["PENNY", 0.5], ["NICKEL", 0], ["DIME", 0], ["QUARTER", 0], ["ONE", 0], ["FIVE", 0], ["TEN", 0], ["TWENTY", 0], ["ONE HUNDRED", 0]]) 
// returns {status: "CLOSED", change: [["PENNY", 0.5], ["NICKEL", 0], ["DIME", 0], ["QUARTER", 0], ["ONE", 0], ["FIVE", 0], ["TEN", 0], ["TWENTY", 0], ["ONE HUNDRED", 0]]}
```
