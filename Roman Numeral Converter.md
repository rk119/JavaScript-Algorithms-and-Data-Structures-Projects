# Roman Numeral Converter

Convert the given number into a roman numeral.

All roman numerals answers should be provided in upper-case.

For better understanding refer to https://www.mathsisfun.com/roman-numerals.html

### Solution :smile:

```javascript
function convertToRoman(num) {
 let romans = {M:1000,CM:900,D:500,CD:400,C:100,XC:90,L:50,XL:40,X:10,IX:9,V:5,IV:4,I:1}
 let romanNum = ""
  for ( let i in romans ) {
    while ( num >= romans[i] ) {
      romanNum += i;
      num -= romans[i];
    }
  }
  return romanNum;
}

convertToRoman(2); // returns II
convertToRoman(36); // returns XXXVI
convertToRoman(649) // returns DCXLIX
```
