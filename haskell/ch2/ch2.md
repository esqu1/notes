Chapter 2 - Starting Out
==

## Ready, set, go!


The Haskell shell is called `ghci`, and can be called as such.

To start off, we can do simple arithmetic in the shell:

```haskell
ghci> 2 + 15
17
ghci> 49 * 100
4900
ghci> 1892 - 1472
420
ghci> 5 / 2
2.5 
ghci> 2 * (15 + 3)
36
```
Boolean operators are the same as in Java:
```haskell
ghci> True && False
False
ghci> True || False
True
ghci> not False
True
```
Equality operators:
```haskell
ghci> 5 == 5
True
ghci> 5 /= 4
True
ghci> "hello" == "hello"
True
```
Some built-in functions:
```haskell
ghci> succ 8
9
ghci> min 9 10
9
ghci> max 100 101
101
ghci> succ 9 + max 5 4 + 1
16
```
These functions are known as *prefix* functions, since the function name goes before the arguments. We can turn these functions into *infix* functions by surrounding the function name with backticks:
```haskell
ghci> div 92 10
9
ghci> 92 `div` 10
9
```
Do note that in Haskell, function application is not denoted with parentheses, like in `foo()`; rather, we use spaces.

## Baby's first functions
We can define functions as follows:
```haskell
doubleMe x = x + x
```
Assume that this is saved in a file called `baby.hs`. To load this function into the `ghci` shell,
```haskell
ghci> :l baby
[1 of 1] Compiling Main             ( baby.hs, interpreted )  
Ok, modules loaded: Main.
ghci> doubleMe 9
18
ghci> doubleMe 8.3
16.6
```
Similarly we can define functions of multiple arguments:
```haskell
doubleUs x y = x*2 + y*2
```
Haskell has an if..else statement, but in Haskell, the else part is mandatory. 
```haskell
doubleSmallNumber' x = (if x > 100 then x else x*2) + 1
```
(The apostrophe at the end usually denotes the "strict" version of a function, since it is a valid character in function names.)

A last thing is that functions cannot begin with capital letters:
```haskell
conanO'Brien = "It's a-me, Conan O'Brien!"   
```

## An intro to lists
Lists in Haskell are *homogenous* data structures, meaning that the list's elements are all of the same type. 
```haskell
ghci> let lostNumbers = [4,8,15,16,23,42]  
ghci> lostNumbers  
[4,8,15,16,23,42] 
```
Concatenation of lists (remember that strings are just lists of characters):
```haskell
ghci> [1,2,3,4] ++ [5,6,7,8]
[1,2,3,4,5,6,7,8]
ghci> "hello" ++ " " ++ "world"  
"hello world"
ghci> ['w','o'] ++ ['o','t']  
"woot"
```
Note here that, just like in Java, characters are represented with single quotes, and strings are represented with double quotes.

We can attach an element to the beginning of a list:
```haskell
ghci> 'A':" SMALL CAT"  
"A SMALL CAT"  
ghci> 5:[1,2,3,4,5]  
[5,1,2,3,4,5]
```
To performing list indexing, use the `!!` operator:
```haskell
ghci> "Steve Buscemi" !! 6  
'B'  
ghci> [9.4,33.2,96.2,11.2,23.25] !! 1  
33.2  
```
Lists are compared lexicographically:
```haskell
ghci> [3,2,1] > [2,1,0]  
True  
ghci> [3,2,1] > [2,10,100]  
True  
ghci> [3,4,2] > [3,4]  
True  
ghci> [3,4,2] > [2,4]  
True  
ghci> [3,4,2] == [3,4,2]  
True  
```
The `head`, `tail`, `last`, and `init` functions are useful for getting specific parts of a list:
```haskell
ghci> head [5,4,3,2,1]  
5 
ghci> tail [5,4,3,2,1]  
[4,3,2,1]  
ghci> last [5,4,3,2,1]  
1   
ghci> init [5,4,3,2,1]  
[5,4,3,2]   
```
A few more functions:
```haskell
ghci> length [5,4,3,2,1] -- returns length of list
5 
ghci> null [1,2,3] -- checks if list is null
False  
ghci> null []  
True 
ghci> reverse [5,4,3,2,1] -- reverses the list
[1,2,3,4,5]
ghci> take 3 [5,4,3,2,1] -- returns list of the first n elements
[5,4,3]  
ghci> take 1 [3,9,3]  
[3]  
ghci> take 5 [1,2]  
[1,2]
ghci> drop 3 [8,4,2,1,5,6] -- returns list with first n elements removed
[1,5,6]  
ghci> drop 0 [1,2,3,4]  
[1,2,3,4]  
ghci> drop 100 [1,2,3,4]  
[]
ghci> minimum [8,4,2,1,5,6]  
1  
ghci> maximum [1,9,2,3,4]  
9  
ghci> sum [5,2,1,6,3,2,5,7]  
31  
ghci> product [6,2,1,2]  
24 
ghci> 4 `elem` [3,4,5,6] -- returns whether first arg is an element of the list
True  
ghci> 10 `elem` [3,4,5,6]  
False 
```

## Texas ranges
Sometimes, we wish to create a list containing a certain discrete range of elements; similar to Python's `range()` function. The syntax for Haskell's equivalent is straightforward:
```haskell
ghci> [1..20]  
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]  
ghci> ['a'..'z']  
"abcdefghijklmnopqrstuvwxyz"  
ghci> ['K'..'Z']  
"KLMNOPQRSTUVWXYZ" 
ghci> [2,4..20] -- indicate a step size with a second element
[2,4,6,8,10,12,14,16,18,20]  
ghci> [3,6..20]  
[3,6,9,12,15,18] 
ghci> [20,19..1] -- reverse must specify step size
[20,19,18,17,16,15,14,13,12,11,10,9,8,7,6,5,4,3,2,1]
ghci> [0.1, 0.3 .. 1]  -- floating point numbers are tricky because of float precision
[0.1,0.3,0.5,0.7,0.8999999999999999,1.0999999999999999]  
```
Infinite lists are also a thing in Haskell; simply do not specify the upper limit. Haskell doesn't evaluate the infinite list in full because it is lazy; it waits for what you want to get out of the list. So `[13,26..24*13]` is the same thing as `take 24 [13,26..]`
```haskell
ghci> take 10 (cycle [1,2,3]) -- cycle will turn a list into a cyclic infinite list
[1,2,3,1,2,3,1,2,3,1]  
ghci> take 12 (cycle "LOL ")  
"LOL LOL LOL "  
ghci> replicate 3 10 -- list with replicated elements
[10, 10, 10]
```

## I'm a list comprehension
Similar to list comprehensions with Python (`[i+3 for i in [1,2,3]]`), we have list comprehensions in Haskell. The syntax for a list comprehension takes on the following form: 
```haskell
[x*2 | x <- [1..10]]
```
Here, `x*2` is the elements that we want, where `x` is derived from `[1..10]`.
```haskell
ghci> [x*2 | x <- [1..10]]  
[2,4,6,8,10,12,14,16,18,20]  
ghci> [x*2 | x <- [1..10], x*2 >= 12] -- have additional conditions (predicates)
[12,14,16,18,20]  
ghci> [ x | x <- [50..100], x `mod` 7 == 3]  
[52,59,66,73,80,87,94]   
```
If statements can be put inside the list comprehension, as follows:
```haskell
boomBangs xs = [ if x < 10 then "BOOM!" else "BANG!" | x <- xs, odd x]
```
```haskell
ghci> boomBangs [7..13]  
["BOOM!","BOOM!","BANG!","BANG!"]
ghci> [ x | x <- [10..20], x /= 13, x /= 15, x /= 19] -- multiple predicates
[10,11,12,14,16,17,18,20] 
ghci> [ x*y | x <- [2,5,10], y <- [8,10,11]] -- draw from several lists
[16,20,22,40,50,55,80,100,110]  
```
We can define a length function:
```haskell
length' xs = sum [1 | _ <- xs]
```
The `_` variable represents a variable that will never be used.
```haskell
ghci> let xxs = [[1,3,5,2,3,1,2,4,5],[1,2,3,4,5,6,7,8,9],[1,2,4,2,1,6,3,1,3,2,3,6]]  
ghci> [ [ x | x <- xs, even x ] | xs <- xxs] -- multiple draws for sublists
[[2,2,4],[2,4,6,8],[2,4,2,6,2,6]]  
```

## Tuples
Tuples are like lists, but their size is immutable, and they do not have to be homogeneous. A thing to note about tuples is that their type is dictated by their elements' types. For example, `(4,5)` is a pair of numbers, while `(4,"hello")` is a pair with a number and a string. Bear this is mind when creating lists of tuples.
```haskell
ghci> fst (8,11) -- returns first component
8  
ghci> fst ("Wow", False)  
"Wow" 
ghci> snd (8,11) -- returns second component
11  
ghci> snd ("Wow", False)  
False
```
One important function is `zip`, which takes two lists and combines them into a list of pairs:
```haskell
ghci> zip [1,2,3,4,5] [5,5,5,5,5]  
[(1,5),(2,5),(3,5),(4,5),(5,5)] 
ghci> zip [5,3,2,6,2,7,2,5,4,6,6] ["im","a","turtle"]  
[(5,"im"),(3,"a"),(2,"turtle")] -- zip cuts off at the shorter list
```
Here is one problem we could solve easily with tuples and list comprehensions: find all right triangles with perimeter less than 24 and all sides integers less than 10.

First, we wish to generate all possible triangles:
```haskell
triangles = [(a,b,c) | a <- [1..10], b <- [1..10], c <- [1..10] ]
```
Next, we check which one of these is a right triangle (adding the condition that a < b < c).
```haskell
rightTriangles = [(a,b,c) | c <- [1..10], b <- [1..c], a <- [1..b], a^2 + b^2 == c^2]
```
Finally, we add the perimeter predicate:
```haskell
rightTriangles' = [(a,b,c) | c <- [1..10], b <- [1..c], a <- [1..b], a^2 + b^2 == c^2, a + b + c < 24]
```