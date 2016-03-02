---
layout: post
title:  "Refactoring to ES6 and back again"
date:   2016-03-02 12:50:13
categories: Refactoring, JavaScript
image:
image-subtitle:
---

A couple of ways of refactoring an ES5 setTimeout function. It needed to be refactored because my linter (airbnb eslint) didn&#0027;t like it, and because it was the right thing to do.

### Original code

Error: func-name Missing function expression name<br>
Changed it to a named function and got...<br>
Error: prefer-arrow-callback Unexpected function expression<br>

    handleReconnect() {
      const reconnectMs = this.reconnectSeconds * 1000;
      console.log(`attempting to reconnect to rabbit. in ${reconnectMs}ms`);
        this.reconnect = setTimeout(function() {
          this.internalConnect();
        }.bind(this), reconnectMs);
    }

### Refactoring

Oook let&#0027;s use a fat arrow callback then.

ES6 fat arrow functions automatically capture the value of *this* from the outer, parent function. The value of this *inside* an arrow function will always be the same as the value of *this* in the arrow’s enclosing function.

There is no need to use `const self = this`, and you can deeply nest arrow functions to preserve *this* through a series of asynchronous operations.

So the above code can be simplified to the below. It is much easier to read and means we do not have to use the bind function to bind the value of this.

    handleReconnect() {
      const reconnectMs = this.reconnectSeconds * 1000;
      console.log(`attempting to reconnect to rabbit. in ${reconnectMs}ms`);
      this.reconnect = setTimeout(() => this.internalConnect(), reconnectMs);
    }

A <a href="https://about.me/riccardocoppola" target="_blank"/>senior dev on my team</a> pointed out that in this case since the internalConnect function was completely defined elsewhere in the file, a callback was not needed. We could just pass the function as an argument.

    handleReconnect() {
      const reconnectMs = this.reconnectSeconds * 1000;
      console.log(`attempting to reconnect to rabbit. in ${reconnectMs}ms`);
      this.reconnect = setTimeout(this.internalConnect.bind(this), reconnectMs);
    }

Nat x
