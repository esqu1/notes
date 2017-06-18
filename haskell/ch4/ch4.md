Chapter 4: Syntax in Functions
==

## Pattern Matching
Pattern matching is a methodology for writing functions. The basic principle is that we specify certain patterns for data to be matched against, and if there is a match, then we dictate what to do with that data (kind of like an if statement w/ regex).

Here is a function that checks if a number is 7:
```haskell
lucky :: (Integral a) => a -> String  
lucky 7 = "LUCKY NUMBER SEVEN!"  
lucky x = "Sorry, you're out of luck, pal!"   
```
When we call `lucky` on a number, the function will evaluate from top down and check against each of the patterns. It will bind to the first pattern if and only if the argument is `7`. If it is not, it will match the second pattern (which matches anything).

Here is a function that says something special for integers between 1 and 5, showing how concise pattern matching is:
```haskell
sayMe :: (Integral a) => a -> String  
sayMe 1 = "One!"  
sayMe 2 = "Two!"  
sayMe 3 = "Three!"  
sayMe 4 = "Four!"  
sayMe 5 = "Five!"  
sayMe x = "Not between 1 and 5"  
```
We can even use Haskell in defining recursions:
```haskell
factorial :: (Integral a) => a -> a  
factorial 0 = 1  
factorial n = n * factorial (n - 1)  
```
Pattern matching can be used in list comprehensions:
```haskell
ghci> let xs = [(1,3), (4,3), (2,4), (5,3), (5,6), (3,1)]  
ghci> [a+b | (a,b) <- xs]  
[4,7,6,8,11,4]  
```
Remember that a list like `[1,2,3]` is the same as `1:2:3:[]`, so we can pattern match this against something like `x:xs` (this is a common pattern in recursion). `x` will bind to the head of the list, and `xs` will bind to the tail.
```haskell
head' :: [a] -> a  
head' [] = error "Can't call head on an empty list, dummy!"  
head' (x:_) = x
```
This gives a modified version of the `head` function.

```haskell
tell :: (Show a) => [a] -> String  
tell [] = "The list is empty"  
tell (x:[]) = "The list has one element: " ++ show x  
tell (x:y:[]) = "The list has two elements: " ++ show x ++ " and " ++ show y  
tell (x:y:_) = "This list is long. The first two elements are: " ++ show x ++ " and " ++ show y
```
We need to ensure that pattern matching checks all cases. In this case, it checks for empty lists, one element lists, 2 element lists, and 3+ element lists.

Finally, we also have *as patterns*, which lets you bind a pattern while keeping a reference to the original data (for example, the head, tail, and the whole list itself).
```haskell
capital :: String -> String  
capital "" = "Empty string, whoops!"  
capital all@(x:xs) = "The first letter of " ++ all ++ " is " ++ [x]  
```
Notice here that `all` will capture the entire list, and `x:xs` capture the head and the tail. 

Also, you cannot use `++` in pattern matching, because that would bring up an ambiguity as to what each of the pattern's components would reference.

## Guards, guards!
Patterns are used to see if a value matches a certain pattern, while guards are used to see if a value meets a certain condition. The syntax is:
```haskell
bmiTell :: (RealFloat a) => a -> String  
bmiTell bmi  
    | bmi <= 18.5 = "You're underweight, you emo, you!"  
    | bmi <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | bmi <= 30.0 = "You're fat! Lose some weight, fatty!"  
    | otherwise   = "You're a whale, congratulations!"  
```
The guards operate on `bmi`, like an if-else statement.
```haskell
bmiTell :: (RealFloat a) => a -> a -> String  
bmiTell weight height  
    | weight / height ^ 2 <= 18.5 = "You're underweight, you emo, you!"  
    | weight / height ^ 2 <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | weight / height ^ 2 <= 30.0 = "You're fat! Lose some weight, fatty!"  
    | otherwise                   = "You're a whale, congratulations!"  
```
Another example, the `max` function:
```haskell
max' :: (Ord a) => a -> a -> a  
max' a b   
    | a > b     = a  
    | otherwise = b  
```

## Where!?

Sometimes, we may find ourselves repeating a certain expression, like the second BMI example above. To relieve this, we can introduce a `where` keyword, to assign an expression to a variable to be used locally in the function:
```haskell
bmiTell :: (RealFloat a) => a -> a -> String  
bmiTell weight height  
    | bmi <= skinny = "You're underweight, you emo, you!"  
    | bmi <= normal = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | bmi <= fat    = "You're fat! Lose some weight, fatty!"  
    | otherwise     = "You're a whale, congratulations!"  
    where bmi = weight / height ^ 2  
          skinny = 18.5  
          normal = 25.0  
          fat = 30.0  
```
We can even use pattern matchings in `where` statements! We could rewrite the above as
```haskell
...  
where bmi = weight / height ^ 2  
      (skinny, normal, fat) = (18.5, 25.0, 30.0)
```
Here's one that returns someone's initials:
```haskell
initials :: String -> String -> String  
initials firstname lastname = [f] ++ ". " ++ [l] ++ "."  
    where (f:_) = firstname  
          (l:_) = lastname   
```
and we can even define function within `where` statements!
```haskell
calcBmis :: (RealFloat a) => [(a, a)] -> [a]  
calcBmis xs = [bmi w h | (w, h) <- xs]  
    where bmi weight height = weight / height ^ 2  
```

## Let it be
Let statements are an alternative to `where` statements, because they can be used anywhere in the function, are expressions themselves, and are very local.
```haskell
cylinder :: (RealFloat a) => a -> a -> a  
cylinder r h = 
    let sideArea = 2 * pi * r * h  
        topArea = pi * r ^2  
    in  sideArea + 2 * topArea  
```
Because of their ability to be expressions, we can do something like this:
```haskell
ghci> 4 * (let a = 9 in a + 1) + 2  
42
ghci> [let square x = x * x in (square 5, square 3, square 2)] -- functions
[(25,9,4)]
ghci> (let a = 100; b = 200; c = 300 in a*b*c, let foo="Hey "; bar = "there!" in foo ++ bar)  
(6000000,"Hey there!") -- multiple let statements inline
ghci> (let (a,b,c) = (1,2,3) in a+b+c) * 100 -- pattern matching
600  
``` 
We can have `let` inside a list comprehension as well:
```haskell
calcBmis :: (RealFloat a) => [(a, a)] -> [a]  
calcBmis xs = [bmi | (w, h) <- xs, let bmi = w / h ^ 2]  
```

## Case expressions
Case expressions are similar to that in C/Java.
```haskell
head' :: [a] -> a  
head' xs = case xs of [] -> error "No head for empty lists!"  
                      (x:_) -> x  
```
The syntax is:
```haskell
case expression of pattern -> result  
                   pattern -> result  
                   pattern -> result  
                   ...  
```