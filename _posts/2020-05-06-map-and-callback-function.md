---
layout: post
title: "淺談 JS map() 與 callback function"
sub_title: "The common elements"
categories:
  - Markup
elements:
  - content
  - css
  - formatting
  - html
last_modified_at: 2020-05-09T23:56:45-05:00
---

本篇懶人包：
> 在 JavaScript 中，像是 map 這種可以把函式當參數傳進函式中運用的函式叫做「高級函式」，如果想要傳進去的參數而不是直接執行的話就要寫成 `[1, 2, 3, 4, 5].map(addOne)` ，而不是 `[1, 2, 3, 4, 5].map(addOne(num))` ，後面這種寫法會讓 addOne 這個函式在 map 被執行前先執行完，而不是對前面的陣列作用。

在 JavaScript 中，`map()` 函式可以將陣列中的每個元素丟到 callback function做計算，這樣就不需要用迴圈了，超方便的！
```js
let new_array = [1, 2, 3, 4, 5].map(i => i + 1);
console.log(new_array);
>> [2, 3, 4, 5, 6]  
```
你看！只要一行就好了！

但方便之前，要先來看看怎麼用。
從[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/map)的說明來看使用方法，
```js
let new_array = arr.map(function callback( currentValue[, index[, array]]) {
    // return element for new_array
}[, thisArg])
```

1. `map` 函式會把回傳給一個新的陣列，所以我們可以用 `new_array` 來接這個回傳值
2. 執行 `map` 函式時，他會呼叫裡面的 callback function ，這個 callback function 會對 `arr` 中的**元素**作用。在這裡要注意的是，callback function 只會對裡面的一個一個元素作用，就好像 `map` 函式已經自帶 for 迴圈一樣，所以我們在寫 callback function 的時候只要在乎我們對單一元素想要做的變化就好。
3. 使用方法裡面的 `[]` 非必填欄位。已 callback function 來說，第一個填進去的參數代表 currentValue，如果你不想處理整個陣列，可以填入第二個參數，代表 index。如果你想要處理二維陣列，那你就可以在參數填入陣列 ^^
4. `{}` 中寫的就是 callback function 的內容

依照上面的使用方法，我們可以寫出這樣的程式碼
```js
let new_array = [1, 2, 3, 4, 5].map(func(i){
  return i = i + 1;
});
console.log(new_array);
>> [2, 3, 4, 5, 6]
```


在 JavaScript 中，有命名函式 `function funcABC(a){}` ，也有匿名函式 `function(a){}`，更有箭號函式 `() => {}` （好像小學照樣照句 www ），所以我們的 callback function 也可以用箭號函式寫：
```js
[1, 2, 3, 4, 5].map((i) => {return i = i + 1})
```

如果參數只有一個可以省略小括弧，如果函式內容只有一行，那就可以省略花括號，所以就可以寫成我們最一開始的樣子，這時 `i + 1` 就成為我們的回傳值
```js
[1, 2, 3, 4, 5].map(i => i + 1)
```

除了上述幾種寫法（已經好多種了），還有一種是把 callback function 定義在函式外面：
```js
function addOne(num){
  return num = num + 1;
}
[1, 2, 3, 4, 5].map(addOne)
```

什麼？為什麼現在 function 放那邊可以不用加參數在後面？
這是我一開始一直搞不清楚的疑問。
在 JavaScript 中，像是 map 這種可以把函式當參數傳進函式中運用的函式叫做「高級函式」，如果想要傳進去的參數而不是直接執行的話就要寫成 `[1, 2, 3, 4, 5].map(addOne)` ，而不是 `[1, 2, 3, 4, 5].map(addOne(num))` ，後面這種寫法會讓 addOne 這個函式在 map 被執行前先執行完，而不是對前面的陣列`[1, 2, 3, 4 ,5]`作用。像這種把函式當成參數丟入另外一個函式，是在 functional programming 中才能做到的事喔！


參考資料：
  1. [Understanding JavaScript’s map() - Discover Meteor](http://www.discovermeteor.com/blog/understanding-javascript-map/)
  2. [Pass a JavaScript function as parameter - Stack Overflow](https://stackoverflow.com/questions/13286233/pass-a-javascript-function-as-parameter)
  3. [Array.prototype.map() - JavaScript  MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
  4. [箭頭函式 - JavaScript  MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
