---
layout: post
title:  "Refactoring to ES6 and back again"
date:   2016-03-02 12:50:13
categories: Refactoring, JavaScript
image:
image-subtitle:
---

I've been doing a lot of linting on a repo that recently got upgraded to Node. This mainly involves changing things to the ES6 syntax since the code was written in ES5. I used airbnb eslint to help me flag stylistic errors and remaining ES5 code.

Here are a couple of ways of refactoring an ES5 setTimeout function which I found particularly interesting.

### Original ES5 code

    handleReconnect() {
      const reconnectMs = this.reconnectSeconds * 1000;
      console.log(`attempting to reconnect to rabbit. in ${reconnectMs}ms`);
        this.reconnect = setTimeout(function() {
          this.internalConnect();
        }.bind(this), reconnectMs);
    }

`Linting error: func-name Missing function expression name`<br>

I changed it to a named function and got...<br>

`Linting error: prefer-arrow-callback Unexpected function expression`<br>


### Refactoring

Oook let's use a fat arrow callback then.

ES6 fat arrow functions automatically capture the value of *this* from the outer, parent function. The value of *this* inside an arrow function will always be the same as the value of *this* in the arrow’s enclosing function.

There is no need to use `const self = this`, and you can deeply nest arrow functions to preserve *this* through a series of asynchronous operations.

So the above code can be simplified to the below ES6 function. It is much easier to read and means we do not have to use the bind function to bind the value of this.

    handleReconnect() {
      const reconnectMs = this.reconnectSeconds * 1000;
      console.log(`attempting to reconnect to rabbit. in ${reconnectMs}ms`);
      this.reconnect = setTimeout(() => this.internalConnect(), reconnectMs);
    }

Then... a <a href="https://about.me/riccardocoppola" target="_blank">senior dev on my team</a> pointed out that in this case since the internalConnect function was completely defined elsewhere in the file (and not changed by handleReconnect), a callback was not actually needed. We could just pass the function as an argument, remembering to bind *this* again since the fat arrow function was no longer being used.

    handleReconnect() {
      const reconnectMs = this.reconnectSeconds * 1000;
      console.log(`attempting to reconnect to rabbit. in ${reconnectMs}ms`);
      this.reconnect = setTimeout(this.internalConnect.bind(this), reconnectMs);
    }

Either way is valid, but the second is a little neater.

Nat x

<img src='https://encrypted-tbn1.gstatic.com/images?q=tbn:ANd9GcQ7ONJR-u9MiHrqeZpMzTqcaik9BrW-XskBAod45N6X1B2mXOGj'/>
