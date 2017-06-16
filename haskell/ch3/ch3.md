Chapter 3 - Types and Typeclasses
==

## Believe the type
Haskell is a statically typed language, which means the type of all objects are known at compile time. This makes code safer so that the program does not crash while running. Furthermore, Haskell has type inference, meaning that we do not have to explicitly tell Haskell the type of a variable. 

To investigate the type of an expression, we use the `:t` command:
```haskell
ghci> :t 'a'  
'a' :: Char  
ghci> :t True  
True :: Bool  
ghci> :t "HELLO!"  
"HELLO!" :: [Char]  
ghci> :t (True, 'a')  
(True, 'a') :: (Bool, Char)  
ghci> :t 4 == 5  
4 == 5 :: Bool  
```
Functions also have their own types, depending on what the types of the inputs and output are. We can declare their type preliminarily:
```haskell
removeNonUppercase :: [Char] -> [Char]  
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]   
```
If we have a function of several parameters:
```haskell
addThree :: Int -> Int -> Int -> Int  
addThree x y z = x + y + z  
```
Here are several different types:
- `Int` is a signed 32-bit integer.
- `Integer` is a signed integer with no bounds. `Int`, however, is more efficient.
- `Float` is a floating point number with single precision.
- `Double` is a floating point number with double precision.
- `Bool` is a boolean.
- `Char` is a character (`'a'`).

## Type variables
When we attempt to check the type of the `head` function, we get that 
```haskell
ghci> :t head  
head :: [a] -> a 
```
`a` is what we refer to as a *type variable*, which are variable that can represent any type, but one specific type only. It is similar to generics in Java. Functions that use type variables are called *polymorphic functions*.

## Typeclasses 101
You can think of typeclasses like interfaces in Java. For example, what happens when view the type for the equality (`==`) operator?
```haskell
ghci> :t (==)  
(==) :: (Eq a) => a -> a -> Bool  
```
We notice the `=>` arrow symbol. Anything that appear to the left of this symbol is a *class constraint*. The function type signature can be read as follows: the equality function takes two values of the same type and returns a boolean. These values must be part of the `Eq` typeclass.

The `Ord` typeclass is similar to the `Comparable` interface in Java; it is for values that have an ordering. `Ordering` is a type that can take on three values: `LT` (less than), `EQ` (equal), and `GT` (greater than).

The `Show` typeclass encompasses values that can be presented as strings. The function `show` actually takes a value of the typeclass `Show` and shows it to us as a string.

The `Read` typeclass is the opposite of `Show`; the `read` function reads a string and returns a member of the typeclass `Read`.
```haskell
ghci> show 3  
"3"  
ghci> show 5.334  
"5.334"  
ghci> show True  
"True" 
ghci> read "True" || False  
True  
ghci> read "8.2" + 3.8  
12.0  
ghci> read "5" - 2  
3  
ghci> read "[1,2,3,4]" ++ [3]  
[1,2,3,4,3]
ghci> read "4"  
<interactive>:1:0:  
    Ambiguous type variable `a' in the constraint:  
      `Read a' arising from a use of `read' at <interactive>:1:0-7  
    Probable fix: add a type signature that fixes these type variable(s)  
```
In the last example, if we simply use the `read` function by itself (which has a type signature of `(Read a) => String -> a`) the function will have no idea what type we wish to return the output in; right now it is a `Read`. To fix this, we can either do something with the result afterwards in the above examples, or we can use *explicit type annotations*.
```haskell
ghci> read "5" :: Int  
5  
ghci> read "5" :: Float  
5.0  
ghci> (read "5" :: Float) * 4  
20.0  
ghci> read "[1,2,3,4]" :: [Int]  
[1,2,3,4]  
ghci> read "(3, 'a')" :: (Int, Char)  
(3, 'a')  
```
The `Enum` typeclass represents values that are sequentially ordered; values we can use in list ranges.
```haskell
ghci> ['a'..'e']  
"abcde"  
ghci> [LT .. GT]  
[LT,EQ,GT]  
ghci> [3 .. 5]  
[3,4,5]  
ghci> succ 'B'  
'C'  
```
The `Bounded` typeclass has values which have bounds:
```haskell
ghci> minBound :: Int  
-2147483648  
ghci> maxBound :: Char  
'\1114111'  
ghci> maxBound :: Bool  
True  
ghci> minBound :: Bool  
False 
```
Note here that minBound and maxBound have type `(Bounded a) => a`, meaning they are polymorphic constants (their type can be a multitude of types).

`Num` is a numeric typeclass, whose members can act like numbers. `Num` members are also polymorphic:
```haskell
ghci> :t 20  
20 :: (Num t) => t 
ghci> 20 :: Int  
20  
ghci> 20 :: Integer  
20  
ghci> 20 :: Float  
20.0  
ghci> 20 :: Double  
20.0  
```
A powerful thing about polymorphic constants is that their properties lead to some great outcomes. For example, the type signature of `(*)` is `(Num a) => a -> a -> a`. This means that the arguments of multiplication must be the same type. Therefore, `(5 :: Int) * (6 :: Integer)` would crash because the two values are of different types. However `5 * (6 :: Int)` would work because `5` is polymorphic, and can act like an `Int`.

`Integral` is a typeclass that only includes `Int` and `Integer`.

`Floating` only includes `Float` and `Double`.

A very useful function is `fromIntegral`, which converts a Integral value to a Num value (it has type signature `(Num b, Integral a) => a -> b`).
