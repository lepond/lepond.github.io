---
layout: post
published: true
title: "Call, Bind, Apply: A Love Story (Part 1)"
mathjax: false
featured: true
comments: false
headline: "Making Angular easier for masses"
categories: 
tags: javascript call
---
Call, Apply, and Bind are three of the most useful (but also misunderstood) built-in, Javascript methods for manipulating context. Over a three-post series, I’ll be explaining how to use each of them. 

Today, we’ll explore how to use .call().
![My helpful screenshot]({{ site.url }}/images/call_love_story.png)

Function.prototype.call(thisArg, [arg1, arg2, arg3])
What is it? 
.call() is a built-in method on the Function prototype. It can only be called on functions.

What does it do?

The simple answer: It calls a function. Taking the function greetChuck as an example, calling greeting() and greeting.call(this) would yield the same result.

var greetChuck = function () {
console.log(“What’s up, Chuck?”);
};

greeting();
greeting.call(this);

The complete(-ish) answer: It calls a function with the context that you specify. In the example above, the function didn’t rely on a specific context (i.e. there was no reference to this in the function), so it wouldn’t make a difference whether we called greeting() and greeting.call(this). 

In fact, the context is the same either way, since we passed this as the argument in .call(). However, there are times when this is referenced in our function, and we want to manipulate what this is. Here’s another example.

We have an object sally, which contains a method greet. 

var sally = {
this.name : ‘Sally’,
this.greet: function () {
console.log(“What’s up, ”+this.name+”?”);
}
}

We also, have an object chuck, but poor chuck has no greet method. 

var chuck = {
this.name: ‘Chuck’
}

How can we call greet on the object chuck? These WON’T work:

chuck.greet(); // chuck.greet() is not a valid method
sally.greet(chuck); // What’s up, Sally?

But using .call() will:

sally.greet.call(chuck); // What’s up, Chuck?

Here’s what’s happening:

The sally.greet method is called, specifically:
function () {
console.log(“What’s up, ”+this.name+”?”);
}
Notice, the actual function doesn’t include Sally’s name directly, just a reference to it through this.name.


The function’s context has been changed by .call(), so now this.name references the object chuck instead of the object sally...even though we used sally’s method.

What about all those other arguments?
You’ll notice that in addition to the required thisArg, there are several optional arguments that can also be included. These are simply a place to list other arguments; the keyword here is list. With .call(), each additional argument is listed just as it would be in a normal function call.

To see this in practice, let’s take a look a modified version of the sally object. (Assume chuck stays the same.)

var sally = {
this.name : ‘Sally’,
this.greet: function () {
console.log(“What’s up, ”+this.name+”?”);
},
this.greetGroup: function (friend1, friend2) {
console.log(“What’s up, ”+this.name+”?”);
console.log(“Great to see you, ”+friend1+”?”);
console.log(“Go home, ”+friend2+”?”);
}
}

Notice the new greetGroup() method on sally, which requires two arguments. We can use the greetGroup() method to greet Sally and her friends Lucy and Peppermint Patty like so:

sally.greet(“Lucy”, “Peppermint Patty”);

Using the additional optional arguments on .call() allow us to call sally.greetGroup() on the object chuck. That way, we can greet Chuck and his friends Linus and Schroeder:

sally.greetGroup.call(chuck, “Linus”, “Schroeder”);

NOTE: .call() is great for manipulating context when you know exactly how many arguments are required; however, there may be times when you won’t know exactly how many other arguments there are. To handle these cases, I’ll explore how to use .apply() in my next post.

What if my function or method doesn’t include a reference to this?
If there’s no reference to this, your function or method would be called as normal, providing it is possible to call that function from the context you passed in. However, it’s important to note that the thisArg is required; if you don’t want to change the context, simply pass in this.

What’s the purpose of manipulating context? 
I’ll get into the finer points of manipulating context in another post, as the reasons for using call, bind, and apply are often quite similar. 

In the examples above, we manipulated context so that one object could “borrow” the methods of another object; this is a pretty common usage scenario. 

Another common usage scenario is in a callback function. This is a sticky, miserable area (that developers hate thinking about) because the context of a callback function is usually not the same as the function that calls it. Binding the value of this to a callback function ensures that it’s called correctly.