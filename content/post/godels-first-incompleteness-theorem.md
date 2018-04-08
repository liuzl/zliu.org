+++
title = "Gödel’s First Incompleteness Theorem for Programmers"

date = 2018-04-08
lastmod = 2018-04-08
math = true
draft = false

tags = ["Logic", "Mathematics", "Philosophy", "Programming"]
summary = "Gödel's incompleteness theorems are two theorems of mathematical logic that demonstrate the inherent limitations of every formal axiomatic system containing basic arithmetic. These results, published by [Kurt Gödel](https://en.wikipedia.org/wiki/Kurt_G%C3%B6del) in 1931, are important both in mathematical logic and in the philosophy of mathematics."

+++

Gödel’s incompleteness theorems have been hailed as “the greatest mathematical discoveries of the 20th century” — indeed, the theorems apply not only to mathematics, but all formal systems and have deep implications for science, logic, computer science, philosophy, and so on. In this post, I’ll give a simple but rigorous sketch of Gödel’s First Incompleteness Theorem. Formally, it states that:

> Any consistent formal system $S$ within which a “certain amount of elementary arithmetic” can be carried out is incomplete; i.e., there are statements of the language of $S$ which can neither be proved nor disproved in $S$.

This is a rigorous proof with a focus on software engineers and programmers. I based this post on this [excellent lecture](https://www.youtube.com/watch?v=9JeIG_CsgvI) I found on YouTube a little while ago. When I first learned how to prove Gödel’s incompleteness theorems, it was in the context of a metalogic class that dealt with all kinds of confusing and deep topics like Henkin construction and transfinitary logic. But as it turns out, Gödel can be understood without much fanfare!

## Introduction

We start on this journey by defining a couple of things. First, we define $F$ as a function that takes a positive integer and returns either `0` or `1`. Here’s an example of such an $F$:

$$
isOdd(x)=\\begin{cases}
0 & x\\text{ is even}\\\\ 
1 & x\\text{ is odd} 
\\end{cases}
$$

So, we see that $isOdd(2)=0$ or that $isOdd(38943981)=1$. We can define $F$ however we want — as long as the output will either be a `0` or a `1`. Let $Q$ be the set of all such functions $F$.

We say that $F$ is computable if there exists a computer program (or proof) $P$ that takes as input $x$ and returns $F(x)$. It goes without saying that $P$ must complete within finite time and must be correct. So let’s look at some code. Is $isOdd(x)$ computable?

```js
function isOdd(x) {
  return (x % 2);
}
```

Looks like it! This program will always return `0` when $x$ is even and `1` when $x$ is odd. What about a more complicated example:

$$
isPrime(x)=\\begin{cases}
0 & x\\text{ is not prime}\\\\ 
1 & x\\text{ is prime} 
\\end{cases}
$$

Is $isPrime$ computable?

```js
function isPrime(x) {
  if(x < 2) return 0;
  if(x == 2) return 1;
  for(var i = 2; i < x; i++) {
    if(x % i === 0) return 0;
  }
  return 1;
}
```

Courtesy of this [Stack Overflow answer](https://stackoverflow.com/a/48356406/243613), it looks like it is. Let $A$ be the set of all computable functions in $Q$. We just found out (the hard way) that $isOdd$ and $isPrime$ are in $A$ — that is, they are computable.

But here’s the big question: are all functions in $Q$ computable? Or, equivalently:

$$
A\stackrel{?}{=}Q
$$

If $A=Q$, then Gödel was wrong, so we need to figure out a clever way to show that $A\subset Q$. In other words, we need to show that there are some functions that take positive integers $x$ as input and return either a `0` or a `1` that we simply **cannot implement**.

## The Set-up

So how do you show that some functions $F$ are not computable? Well, let’s do it the old-fashioned way. Since we’re already using JavaScript, let’s just print out every single JavaScript program. Ever. To make things easy, we can order them alphabetically and by length (in [lexicographical order](https://en.wikipedia.org/wiki/Lexicographical_order)). To make things even easier, we can just throw out programs that loop infinitely or don’t return a `1` or a `0`. When all is said and done, we’re left with an infinite number of programs that probably start out like this:

```js
function F1(x) {
  return 0;
}
```
```js
function F2(x) {
  return 1;
}
```

And further down the line…
```js
function Fn(x) {
  return 1 - 1;
}
```

And further down the line…
```js
function Fn(x) {
  return x / x;
}
```

And further down the line…
```js
function Fn(x) {
  return (x % 2); // hey, this is the isOdd function from before!
}
```

And even further down the line…
```js
function Fn(x) {
  return (x % 2) / 1;
}
```

And even further down the line…
```js
function Fn(x) {
  let someRandomVariable = x ^ x;
  let abcd = someRandomVariable / y;
  if (abdc > -12) return 0;
  return 1;
}
```

You get the picture. Now we have every single possible program written in JavaScript that outputs `0` or `1`. In other words, we just populated $A$: we now have a program that goes with every single computable function. Let’s put them in a big table called $T$. The column headings indicate inputs (positive integers) and the rows indicate computable functions (and, implicitly, the programs that implement them).

||1|2|3|4|5|6|7|...|n|...|
|---|---|---|---|---|---|---|---|---|---|---|
|$f\_1$|$f\_1(1)$|$f\_1(2)$|$f\_1(3)$|$f\_1(4)$|$f\_1(5)$|$f\_1(6)$|$f\_1(7)$|...|$f\_1(n)$|...|
|$f\_2$|$f\_2(1)$|$f\_2(2)$|$f\_2(3)$|$f\_2(4)$|$f\_2(5)$|$f\_2(6)$|$f\_2(7)$|...|$f\_2(n)$|...|
|...|
|$f\_{o}$|1|0|1|0|1|0|1|...|$f\_{o}(n)$|...|
|...|
|$f\_{p}$|0|1|1|0|1|0|1|...|$f\_{p}(n)$|...|
|...|
|$f\_i$|$f\_i(1)$|$f\_i(2)$|$f\_i(3)$|$f\_i(4)$|$f\_i(5)$|$f\_i(6)$|$f\_i(7)$|...|$f\_i(n)$|...|
|...|

You’ll notice that $isOdd$ and $isPrime$ also made it in our table (as fo and fp, respectively). So far, so good. It seems that we thought of everything. But let’s define a new function:

$$
\bar{f}(i)=1-f\_i(i)
$$

Where $f\_i$ is the $i$th function in table $T$. First, let’s make sure we’re convinced that $\bar{f}$ is well-formed. The input $i$ is an integer. $f\_{i}(i)$ will return either a `0` or a `1`, given that $f\_i$ has a row populated in $T$. And finally, $1–0= 1$ or $1–1=0$, so both cases are well-formed.

$$
\bar{f}(i)=\stackrel{\text{will return }0\text{ or }1}{\overbrace{1-\underset{\text{will return 0 or 1}}{\underbrace{f\_{i}(i)}}}}
$$

Therefore, $\bar{f}$ is in $Q$, but is it in $T$?

## The Proof

Seeing why $\bar{f}$ can’t be in $T$ is pretty straightforward. Suppose $\bar{f}$ is $f\_2$. Like we’ve seen so far, $f\_2(2)$ can either be `0` or `1`. But if $f\_2(2)=0$, then $\bar{f}(2)=1−f\_2(2)=1−0=1$. So, we have:

$$
f\_2(2) \neq \bar{f}(2)
$$

Whoops. Okay, so it wasn’t $f\_2$. That would be too easy. What about $f\_{421}$? If $f\_{421}(421)=1$, then $\bar{f}(421)=1−f\_{421}(421)=1−1=0$. So, we have:

$$
f\_{421}(421) \neq \bar{f}(421)
$$

As it turns out, any computable function we pick out of $T$ (and implicitly $A$) will disagree with $\bar{f}$ at at least one output. And therefore, we have the amazing finding that:

$$
A\subset Q
$$

## Conclusion

In other words, there are some things, like $\bar{f}$, we can’t prove or disprove in formal systems. Given that we’ve been working with JavaScript in this post, it makes sense that it’s impossible to reference the program’s own lexicographic index in $T$. With that said, you might be tempted to think that there might be a way to “get around” this limitation; if your language was clever enough, perhaps. I’ll eventually write about Gödel’s Second Incompleteness Theorem, which drives the nail in the coffin: there’s no way to get around this.
