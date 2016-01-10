o today I wrote this on Twitter:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">People still think that Reactive libs/frameworks are just an enhanced KVO. #2016</p>&mdash; Rui Peres (@peres) <a href="https://twitter.com/peres/status/685157803095388160">January 7, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

######Part 1

And I got some feedback from [Miguel Ferreira](https://twitter.com/amiguelferreira). Since, in general, I agree with his opinions about software development, I decided to write this post. His arguments were in Portuguese, so I will give him justice by highlighting them[^n]:

1. FRP is useful in some cases, but in many others, it's just more difficult to understand about what's going versus an imperative approach.
2. An important aspect of any programming paradigm is how easy is to understand the code.
3. From my understanding FRP is very useful for forms/input validation.

I think the general idea here is: FRP is difficult to understand. And I agree. It takes some effort to understand what's going on. Most people come from an imperative background so there is an initial cognitive overload. 

What I have found though, while using it for almost two years, is that an apps's state becomes simpler to manage. Not only that, complex systems are easily expressible with FRP's primitives. 

> From my understanding FRP is very useful for forms/input validation.

Well that's pretty much **the** canonical example. One is able to show off in a couple of lines:

1. Observables
2. Transformations
3. Composition

A person that never used a FRP framework/lib easily gets this example ingrained in his/her mind. But, it might assume that FRP is only good for that. 🙁 

######Conclusion

Just because something is easy doesn't necessarily make it simple[^n]. 

----
######Part 2

Next, I showed something I did at [NSCoimbra](https://github.com/NSCoimbra/ReactiveDemo). The specific part picked up by [Miguel](https://twitter.com/amiguelferreira) was [this](https://github.com/NSCoimbra/ReactiveDemo/blob/master/DemoCoimbra/Components/Persistence/Persistence.swift#L28#L43)[^n]. And his comments highlighted below:

1. **Asynchronous:** What's the advantage of using Signals versus dispatch+blocks[^n]?
2. **Transformation + Composition**: In that method, I can't see anything specific in reactive that benefits from composition or transformations versus an imperative, deterministic approach. At the end of the day you know that either comes a `NSData` object or an `Error`. To conclude, where I really see reactive shinning: is when you have to deal with multiple sources that send values in a non-deterministic manner. 
3. So the only useful thing here is how reactive deals with async

> What's the advantage of using Signals versus dispatch+blocks[^n]?

GCD has composition issues. A quick example is `NSOperation`, which is an abstraction on top of GCD, that suffers from the same problem[^n]. 

Let's imagine a scenario, where you have three `NSOperations` that encapsulates the following pieces of work:

1. Networking
2. Parsing
3. Persistence

How would you architect this? How do you pass values from one `NSOperation` to another? What happens if one `NSOperation` fails, how is the error handled? You might get fancy and end up with [this](https://developer.apple.com/sample-code/wwdc/2015/downloads/Advanced-NSOperations.zip). 🙈

On the other hand, even if you don't understand FRP, its primitives, operations, Monads and `flatMaps`. I bet you would be able to understand what this does:

<script src="https://gist.github.com/RuiAAPeres/8a72527e5702855edc0d.js"></script>

> I can't see anything specific in reactive that benefits from composition or transformations versus an imperative, deterministic approach.

Being deterministic (being able to get always the same output from the same initial state/conditions) in this case, it's orthogonal to the approach used (FRP or Imperative)

> At the end of the day you know that either comes a `NSData` object or an `Error`.

You have that in both Imperative and FRP. What matters here, at least in my opinion, is how the output is handled.

> So the only useful thing here is how reactive deals with async

It's true that FRP libs/frameworks help dealing with asynchronous work. You still need to be careful where you subscribe, or start signals tho. Since you can easily block the main thread. Nevertheless, as showed above, FRP is more than that. 

######Conclusion

FRP libs/frameworks are more than just a fancy KVO, but it also goes beyond input validation. It's a way to handle, derive and manage state in a simple unified way. It's easy to just dismiss FRP, because, as previously mentioned, it might be a bit difficult in the beginning, but it's very much worth the initial trouble. ✨


[^n]: I used the term Reactive on Twitter and during the conversation, but what I meant was Functional Reactive Programming. [But let's not get lost in the details.](http://stackoverflow.com/a/5386908/491239)
[^n]: [Nacho Soto](https://twitter.com/NachoSoto), does a far better job talking about it [here](https://realm.io/news/nacho-soto-functional-reactive-programming/) than me.
[^n]: I was moving from an app without ReactiveCocoa to one with it. So I wrapped the [previous implementation](https://github.com/NSCoimbra/ReactiveDemo/blob/master/DemoCoimbra/Components/Persistence/Persistence.swift#L74#L86), instead of creating one from scratch.
[^n]: I assume when Miguel mentioned "dispatch+block" he meant the use of GCD. 
