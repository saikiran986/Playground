Back to home 

## **Learn Callbacks** 

February 20, 2026 at 15:39 115 views 

**==> picture [516 x 650] intentionally omitted <==**

## **JavaScript Callbacks, setTimeout & setInterval** 

## **(Without Promises / async-await)** 

This guide explains: 

- What callbacks are 

- Proper error handling (Node.js style) 

- Using Cl) setTimeout 

- Using CT) setInterval 

- Avoiding common mistakes 

- Good practices 

Everything is written clearly and simply. 

## **What is a Callback?** 

A **callback** is a function passed into another function and executed later. 

**==> picture [289 x 606] intentionally omitted <==**

**----- Start of picture text -----**<br>
JS<br>1 function greet(name, callback) {<br>2 console.log("Hello " + name);<br>3 callback();<br>4 }<br>5<br>6 function afterGreeting() {<br>7 console.log("Greeting completed.");<br>8 }<br>9<br>10 greet("Gangadhar", afterGreeting);<br> Good Practice: Validate Callback<br>Always check whether it’s a function.<br>JS<br>1 function greet(name, callback) {<br>2 console.log("Hello " + name);<br>3<br>4 if (typeof callback === "function") {<br>5 callback();<br>6 }<br>7 }<br>**----- End of picture text -----**<br>


## **Good Practice: Validate Callback** 

Always check whether it’s a function. 

## **Callback With Parameters** 

## Callbacks can receive data. 

JS **1 function calculate(a, b, callback) { 2 var sum = a + b; 3 callback(sum); 4 } 5 6 calculate(5, 3, function(result) { 7 console.log("Sum is:", result); 8 });** 

## **Node.js Style Error-First Callback** 

Standard pattern: 

callback(error, result) 

- First argument → error 

- Second argument → result 

JS **1 function divide(a, b, callback) { 2 if (b === 0) { 3 return callback("Cannot divide by zero", null); 4 } 5 6 var result = a / b; 7 callback(null, result); 8 } 9 10 divide(10, 2, function(error, result) { 11 if (error) { 12 console.error("Error:", error); 13 } else { 14 console.log("Result:", result); 15 } 16 }); Important Rule** Always return when sending an error. JS **if (b === 0) { return callback("Error", null); }** ~~a~~ 

## **Simulating Async Operation Using setTimeout** 

JS 

**1 function fetchData(callback) { 2 console.log("Fetching data..."); 3 4 setTimeout(function() { 5 var data = { id: 1, name: "John" }; 6 callback(null, data); 7 }, 2000); 8 } 9 10 fetchData(function(error, data) { 11 if (error) { 12 console.error(error); 13 } else { 14 console.log("Received:", data); 15 } 16 });** 

## **Handling Errors Inside setTimeout** 

Asynchronous errors must be handled inside the callback. 

## JS **1 setTimeout(function() { 2 try { 3 var result = JSON.parse("invalid json"); 4 console.log(result); 5 } catch (error) { 6 console.error("Parsing failed:", error.message); 7 } 8 }, 1000);** ~~=~~ **Using setTimeout** Runs once after delay (milliseconds). JS **1 console.log("Start"); 2 3 setTimeout(function() { 4 console.log("Executed after 2 seconds"); 5 }, 2000); 6 7 console.log("End");** ~~—~~ **Cancel setTimeout** 

**==> picture [285 x 122] intentionally omitted <==**

**----- Start of picture text -----**<br>
JS<br>1 var timeoutId = setTimeout(function() {<br>2 console.log("This will not execute");<br>3 }, 5000);<br>4<br>5 clearTimeout(timeoutId);<br>**----- End of picture text -----**<br>


## **Using setInterval** 

Runs repeatedly at fixed interval. 

**==> picture [289 x 232] intentionally omitted <==**

**----- Start of picture text -----**<br>
JS<br>1 var count = 0;<br>2<br>3 var intervalId = setInterval(function() {<br>4 count++;<br>5 console.log("Count:", count);<br>6<br>7 if (count === 5) {<br>8 clearInterval(intervalId);<br>9 console.log("Interval stopped");<br>10 }<br>11 }, 1000);<br>**----- End of picture text -----**<br>


**Important: Always Clear Intervals** 

If you don't: 

- Memory leaks can happen 

- Unnecessary CPU usage 

- Unexpected repeated execution 

## **Callback Inside setTimeout** 

JS **1 function delayedMessage(message, callback) { 2 setTimeout(function() { 3 console.log(message); 4 callback(); 5 }, 1000); 6 } 7 8 delayedMessage("Hello after delay", function() { 9 console.log("Done"); 10 });** 

# **Callback Hell (Problem Example)** 

JS 

**1 setTimeout(function() { 2 console.log("Step 1"); 3 4 setTimeout(function() { 5 console.log("Step 2"); 6 7 setTimeout(function() { 8 console.log("Step 3"); 9 }, 1000); 10 11 }, 1000); 12 13 }, 1000);** 

## **Better Structure Without Promises** 

|||||||||||||||||||JS|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
||||||||||||||||||||
||||||||||||||||||||
||**1**||||**function** **step1(callback)** **{**||||||||||||||
||**2**||||||||**setTimeout(function()**|||||**{**|||||
||||||||||||||||||||
||**3**||||||||**console.log("Step 1");**|||||**console.log("Step 1");**|||||
||**4**||||||||**callback();**||||||||||
||||||||||||||||||||
||**5**||||||||**},** **1000);**||||||||||
||||||||||||||||||||
||**6**||||**}**||||||||||||||
||||||||||||||||||||
||**7**||||||||||||||||||
||||||||||||||||||||
||**8**||||**function** **step2(callback)** **{**||||||||||||||
||**9**||||||||**setTimeout(function()**|||||**{**|||||
||||||||||||||||||||
|**10**|||||||||**console.log("Step 2");**|||||**console.log("Step 2");**|||||
|**11**|||||||||**callback();**||||||||||
||||||||||||||||||||
|**12**|||||||||**},** **1000);**||||||||||
||||||||||||||||||||
|**13**|||||**}**||||||||||||||
||||||||||||||||||||
|**14**|||||||||||||||||||
||||||||||||||||||||
|**15**|||||**function** **step3()** **{**||||||||||||||
|**16**|||||||||**setTimeout(function()**|||||**{**|||||
||||||||||||||||||||
|**17**|||||||||**console.log("Step 3");**|||||**console.log("Step 3");**|||||
|**18**|||||||||**},** **1000);**||||||||||
||||||||||||||||||||
|**19**|||||**}**||||||||||||||
||||||||||||||||||||
|**20**|||||||||||||||||||
||||||||||||||||||||
|**21**|||||**step1(function()** **{**||||||||||||||
|**22**|||||||||**step2(function()** **{**||||||||||
|**23**|||||||||**step3();**||||||||||
|**24**|||||||||**});**||||||||||
||||||||||||||||||||
|**25**|||||**});**||||||||||||||



## **setInterval with Error Handling** 

JS 

- **1 var counter = 0;** 

**2** 

- **3 var id = setInterval(function() { 4 try {** 

- **5 counter++; 6 console.log("Counter:", counter); 7 8 if (counter === 3) {** 

- **9 throw new Error("Something went wrong");** 

- **10 } 11 12 if (counter === 5) { 13 clearInterval(id); 14 }** 

- **15** 

- **16 } catch (error) { 17 console.error("Error occurred:", error.message); 18 clearInterval(id);** 

- **19 }** 

- **20 }, 1000);** 

# **Avoid Multiple Callback Calls (Common Mistake)** 

Bad: 

## JS **function test(callback) { callback(); callback(); }** Safe approach: JS **1 function test(callback) { 2 var called = false; 3 4 if (!called) { 5 called = true; 6 callback(); 7 }** ~~[a]~~ **8 }** ~~—~~ **Summary of Best Practices** ✔ Always validate callbacks ✔ Follow error-first pattern ✔ Always return after error callback ✔ Clear intervals when done ✔ Store timeout/interval IDs ✔ Use try-catch inside async blocks ✔ Avoid deep nesting (split functions) ✔ Never call callback multiple times 

Create your own document 

