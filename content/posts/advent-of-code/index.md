---
title: Advent of Code 2023
date: 2024-01-01T00:00:00-04:00
description: Advent Calendar Contest 2023
slug: advent-of-code
categories:
    - String Parsing
    - Competitive Programming
tags:
    - Haskell
weight: 1
---

# Intro

This year's advent of code was interesting. I decided this was the year I would
start learning Haskell, since I had gotten my hands on the "Category theory for Programmers"
book by Bartosz Milewski. Not withstanding, this year featured an entire week of
string parsing and manipulation. Because of this, this year has to be one of the
most tedious advent of code years yet.

I had just gotten my hands on "Category Theory for Programmers" by Bartosz
Milewski. This book is phenomenal! It introduces category theory concepts in a
very digestible way. One of the category theory concepts that the book exposes
is function composition. One way to compose functions is using function
combinators, for example the famous [combinator birds](https://www.angelfire.com/tx4/cus/combinator/birds.html).
These combinator birds express different ways of composing functions using a
single function. Which lets you build your own custom complex algorithm by
merely picking and choosing which functions you want to use.

# Day 1

Day one started with string parsing. They give you an input that has both number
words and digits and you have to pick the first digit that appears from both the
right and the first from the left.

My solution to the first part was the following:
```haskell
import Data.List
import Data.Char

toInt = read :: String -> Int

starling = \a b c d -> a (b d) (c d)

toList x1 x2 = [x1, x2]

getNum = starling toList head last . filter isDigit

solve1 = sum . map (toInt . getNum)

main = interact $ show . solve1 . words
```

This solution was somewhat concise. You have the starling combinator, which
admittedly is not descriptive and a very bad name, which you use to combine the
head and last of the digits in the input.

The second part was not as simple. The second part asks that you parse each word
and find what the first and what the last possibly digit or word number is.
- for example: `twoeight3` would be "283"

```haskell
import Data.List
import Data.Char

toInt = read :: String -> Int

starling = \a b c d -> a (b d) (c d)

number "one" = "1"
number "two" = "2"
number "three" = "3"
number "four" = "4"
number "five" = "5"
number "six" = "6"
number "seven" = "7"
number "eight" = "8"
number "nine" = "9"
number "1" = "1"
number "2" = "2"
number "3" = "3"
number "4" = "4"
number "5" = "5"
number "6" = "6"
number "7" = "7"
number "8" = "8"
number "9" = "9"
number _ = ""

splitAll [] = []
splitAll (x:xs) = [x] : splitAll xs

prefixes = scanl1 (++) . splitAll

suffixes = scanr1 (++) . splitAll

allSubstrs = foldl' (++) [""] . map (suffixes) . prefixes

getNum = starling (++) head last . filter (/= "") . map number . allSubstrs

solve2 = sum . map (toInt . getNum)

main = interact $ show . solve2 . words
```

That's a lot to digest.
