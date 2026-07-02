JavaScript Callbacks, setTimeout & setInterval
(Without Promises / async-await)

This guide explains:

    What callbacks are
    Proper error handling (Node.js style)
    Using setTimeout
    Using setInterval
    Avoiding common mistakes
    Good practices

Everything is written clearly and simply.
1️⃣ What is a Callback?

A callback is a function passed into another function and executed later.

js

    function greet(name, callback) {
        console.log("Hello " + name);
        callback();
    }
    
    function afterGreeting() {
        console.log("Greeting completed.");
    }
    
    greet("Gangadhar", afterGreeting);

✅ Good Practice: Validate Callback

Always check whether it’s a function.

js

    function greet(name, callback) {
        console.log("Hello " + name);
    
        if (typeof callback === "function") {
            callback();
        }
    }

2️⃣ Callback With Parameters

Callbacks can receive data.

js

    function calculate(a, b, callback) {
        var sum = a + b;
        callback(sum);
    }
    
    calculate(5, 3, function(result) {
        console.log("Sum is:", result);
    });

3️⃣ Node.js Style Error-First Callback

Standard pattern:

callback(error, result)

    First argument → error
    Second argument → result

js

    function divide(a, b, callback) {
        if (b === 0) {
            return callback("Cannot divide by zero", null);
        }
    
        var result = a / b;
        callback(null, result);
    }
    
    divide(10, 2, function(error, result) {
        if (error) {
            console.error("Error:", error);
        } else {
            console.log("Result:", result);
        }
    });

✅ Important Rule

Always return when sending an error.

js

    if (b === 0) {
        return callback("Error", null);
    }

4️⃣ Simulating Async Operation Using setTimeout

js

    function fetchData(callback) {
        console.log("Fetching data...");
    
        setTimeout(function() {
            var data = { id: 1, name: "John" };
            callback(null, data);
        }, 2000);
    }
    
    fetchData(function(error, data) {
        if (error) {
            console.error(error);
        } else {
            console.log("Received:", data);
        }
    });

5️⃣ Handling Errors Inside setTimeout

Asynchronous errors must be handled inside the callback.

js

    setTimeout(function() {
        try {
            var result = JSON.parse("invalid json");
            console.log(result);
        } catch (error) {
            console.error("Parsing failed:", error.message);
        }
    }, 1000);

6️⃣ Using setTimeout

Runs once after delay (milliseconds).

js

    console.log("Start");
    
    setTimeout(function() {
        console.log("Executed after 2 seconds");
    }, 2000);
    
    console.log("End");

✅ Cancel setTimeout

js

    var timeoutId = setTimeout(function() {
        console.log("This will not execute");
    }, 5000);
    
    clearTimeout(timeoutId);

7️⃣ Using setInterval

Runs repeatedly at fixed interval.

js

    var count = 0;
    
    var intervalId = setInterval(function() {
        count++;
        console.log("Count:", count);
    
        if (count === 5) {
            clearInterval(intervalId);
            console.log("Interval stopped");
        }
    }, 1000);

✅ Important: Always Clear Intervals

If you don't:

    Memory leaks can happen
    Unnecessary CPU usage
    Unexpected repeated execution

8️⃣ Callback Inside setTimeout

js

    function delayedMessage(message, callback) {
        setTimeout(function() {
            console.log(message);
            callback();
        }, 1000);
    }
    
    delayedMessage("Hello after delay", function() {
        console.log("Done");
    });

9️⃣ Callback Hell (Problem Example)

js

    setTimeout(function() {
        console.log("Step 1");
    
        setTimeout(function() {
            console.log("Step 2");
    
            setTimeout(function() {
                console.log("Step 3");
            }, 1000);
    
        }, 1000);
    
    }, 1000);

🔟 Better Structure Without Promises

js

    function step1(callback) {
        setTimeout(function() {
            console.log("Step 1");
            callback();
        }, 1000);
    }
    
    function step2(callback) {
        setTimeout(function() {
            console.log("Step 2");
            callback();
        }, 1000);
    }
    
    function step3() {
        setTimeout(function() {
            console.log("Step 3");
        }, 1000);
    }
    
    step1(function() {
        step2(function() {
            step3();
        });
    });

1️⃣1️⃣ setInterval with Error Handling

js

    var counter = 0;
    
    var id = setInterval(function() {
        try {
            counter++;
            console.log("Counter:", counter);
    
            if (counter === 3) {
                throw new Error("Something went wrong");
            }
    
            if (counter === 5) {
                clearInterval(id);
            }
    
        } catch (error) {
            console.error("Error occurred:", error.message);
            clearInterval(id);
        }
    }, 1000);

1️⃣2️⃣ Avoid Multiple Callback Calls (Common Mistake)

❌ Bad:

js

    function test(callback) {
        callback();
        callback();
    }
    
  ✅ Safe approach:
    
    js
    
    function test(callback) {
        var called = false;
    
        if (!called) {
            called = true;
            callback();
        }
    }

🔥 Summary of Best Practices

✔ Always validate callbacks ✔ Follow error-first pattern ✔ Always return after error callback ✔ Clear intervals when done ✔ Store timeout/interval IDs ✔ Use try-catch inside async blocks ✔ Avoid deep nesting (split functions) ✔ Never call callback multiple times
