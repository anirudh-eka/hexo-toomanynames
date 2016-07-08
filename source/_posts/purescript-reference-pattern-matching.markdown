---
layout: post
title: "Purescript Reference: Pattern Matching" 
categories: 
- code
tags: 
- haskell 
- javascript 
- frontend 
- functional
share: discuss
date: 2016-01-27
---




This is a reference guide for how Purescript pattern matching works. Almost all of it was distilled from [PureScript by Example](https://leanpub.com/purescript/read) by Phil Freeman the original developer of the Purescript compiler. I highly recommend reading it for in depth stuff. This is just for reference. 

<span style='display: none;'><!--more--></span>

Simple Patterns
-----------------------
There are many types of simple patterns. 
variable patterns  are ones that bind arguments to a name like so:

{% codeblock lang:haskell %}

	add x y = x + y
{% endcodeblock %}

wild card patterns matches any argument like a variable pattern, but does not bind a name to it

{% codeblock lang:haskell %}

	avoidArgument _ = "blah blah"
{% endcodeblock %}

literal patterns match to specific values for example here are some Int, String, and Boolean literals:

{% codeblock lang:haskell %}

	addOneWithFour 1 4 = 1 + 4 
	reply "hi" = "why are you talking to me?"
	toString true = "true"
{% endcodeblock %}
	
Guards
-----------
Purescript supports guards like lang:haskell. Here is an example for the Euclidean algorithm:
{% codeblock lang:haskell %}

	gcd :: Int -> Int -> Int
	gcd n 0 = n
	gcd 0 n = n
	gcd n m | n > m =gcd(n-m)m
		      | otherwise = gcd n (m - n)
{% endcodeblock %}

Array Patterns
----------------------
In Purescript, you can apply some of the same patterns you can do on a lang:haskell list. The purescript Array is based on the JS Array, except it only take entries of a single type. PS also has a linked list that can be found in Data.List, which is like the lang:haskell List. This one is much more efficient. Anyway I digress! Here is some ways you can pattern match on an Array:

{% codeblock lang:haskell %}

	isEmpty :: forall a. Array a -> Boolean
	isEmpty [] = true
	isEmpty _ = false
	
	takeFive :: Array Int -> Int
	takeFive [1, 2, a, b, _] = a * b
	takeFive _ = 0

{% endcodeblock %}

*Note: Purescript does not support pattern matching with `cons (:)` operator on `Data.Array`! This is due to poor performance. :(  You can use `Cons` for pattern matching on Data.List using `Cons`*

Record Patterns
-----------------------
Record patterns are used to match records. Recall a record is a light weight data structure, like so:
algorithm:

{% codeblock lang:haskell %}
	
	type Person = {firstName :: String, lastName :: String}

{% endcodeblock %}

We can use record patterns to match records, like so:

{% codeblock lang:haskell %}

	type Person = {firstName :: String, lastName :: String}
	
	showPerson :: Person -> String
	showPerson { firstName: "Anirudh", lastName: y} =  "Anirudh, the baddest"
	showPerson { firstName: x, lastName: y} =  y ++ ", " ++ x

{% endcodeblock %}

*Note: If we don't want to explicitly create a 'Person' type, we can also define `showPerson` as such*

{% codeblock lang:haskell %}

	showPerson :: { firstName :: String, lastName :: String }  -> String
	showPerson { firstName: "Anirudh", lastName: y } =  "Anirudh, the baddest"
	showPerson { firstName: x, lastName: y } =  y ++ ", " ++ x

{% endcodeblock %}

Row Polymorphism
----------------------------
Row polymorphism is the idea that a function can take a record with a variable number of rows, as long as the explicitly defined rows are matched. To give row polymorphism in the example above:

{% codeblock lang:haskell %}

	showPerson :: forall r. {firstName :: String, lastName :: String | r } -> String
	showPerson { firstName: "Anirudh", lastName: y} =  "Anirudh, the baddest"
	showPerson { firstName: x, lastName: y} =  y ++ ", " ++ x

{% endcodeblock %}

Now you can do this:

{% codeblock lang:haskell %}

	> showPerson { firstName: "Anirudh", lastName: "Dhullipalla", nickname: "rudy"}
	"Anirudh, the baddest"

{% endcodeblock %}

But not this: 

{% codeblock lang:haskell %}

	> showPerson { firstName: "Anirudh"}
	Error found: ...
	
{% endcodeblock %}

Nested Patterns
-----------------------
Nested Patterns are exactly what you think. Array and Record patterns use them, but you can make them deep, like so:

{% codeblock lang:haskell %}

	type Address = { street :: String, city :: String }
	
	type Person = { name :: String, address :: Address }
	
	livesInLA :: Person -> Boolean
	livesInLA { address: { city: "Los Angeles" } } = true 
	livesInLA _ = false

{% endcodeblock %}

Named Patterns
-----------------------
You can bring additional names into scope with nested patterns by naming the array itself:

{% codeblock lang:haskell %}

	sortPair :: Array Int -> Array Int 
	sortPair arr@[x, y]
		| x <= y = arr
		| otherwise = [y, x]

{% endcodeblock %}

Here we have named the Array itself as well as pieces of its content. 

Algebraic Datatypes
-----------------------------
Algebraic Datatypes offers the ability to maintain modularity while extending the functionality of an abstraction. It's helpful to understand it's benefit by comparing it with OO. Imagine we are working with shapes and we wanted to represent some common functionality all shapes will have. In an OO language we might do this:

{% codeblock java %}

	interface Shape {
		area() 
	} 
	
	class Circle : Shape {
		area() {
			...
		}	
	}
	
	class Rectangle : Shape {
		area() {
			...
		}	
	}

{% endcodeblock %}

Now imagine we decided that shapes should support perimeter. If we add `perimeter()` to `Shape` then we would be forced to modify all of the classes that implement `Shape`. This in a way breaks modularity.

With algebraic data types, we:
 
1. capture the various types of shapes in the datatype definition (use the `data` keyword to define a datatype):{% codeblock lang:haskell %} data Shape = Circle Point Number | Rectangle Point Number Number
{% endcodeblock %}

2. and then add functionality to each of the shapes using pattern matching:

{% codeblock lang:haskell %}
	area :: Shape -> Number
	area (Circle _ Number) = Number * Number * Pi
	area (Rectangle Point Number Number) = Number * Number
{% endcodeblock %}

Notice you have access to the data you used to construct the shape with pattern matching. Neat, huh? It makes you think about what a concrete representation of an abstract idea really is. Really, what makes concrete representations of a single type different is the data needed to make them and their name.
