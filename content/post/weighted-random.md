+++
title = "Weighted Random: algorithms for sampling from discrete probability distributions"

date = 2018-04-05
lastmod = 2018-04-06
math = true
draft = false
highlight_languages = ["go"]

tags = ["algorithm", "sampling", "alias method", "weighted random"]
summary = "The optimal solution for weighted random should be the [Alias Method](https://en.wikipedia.org/wiki/Alias_method). It requires $O(n)$ time to initialize, $O(1)$ time to make a selection, and $O(n)$ memory."

[header]
image = "random_post_img.png"
+++

## Introduction

First of all what is weighted random? Let's say you have a list of items and you want to pick one of them randomly. Doing this seems easy as all that's required is to write a litte function that generates a random index referring to the one of the items in the list. But sometimes plain randomness is not enough, we want random results that are biased or based on some probability. This is where the weighted random generation algorithm needed.

## Scenarios

There are lots of real world scenarios that need weighted random. Such as load balancers(like [nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/), haproxy etc). Following is an example configuration of nginx. In the example, `backend1.example.com` has weight `5`; the other two servers have the default weight `1`, With this configuration of weights, out of every seven requests, five are sent to `backend1.example.com` and one to `backend2.example.com` one to `backend3.example.com`.
```nginx
http {
    upstream backend {
        server backend1.example.com weight=5;
        server backend2.example.com;
        server backend3.example.com;
    }
}
```

Another example is crawler scheduling. When I was developing a concurrent [crawling framework](https://github.com/crawlerclub/x) last year, I need to schedule the crawling tasks according to the task importenceness. The tasks importenceness are expressed by float value weights that are mannually assigned to each site that tasks belong to. So there should be a [WeightedChoice](https://github.com/crawlerclub/x/blob/master/controller/crawler_scheduler.go#L45) function on the scheduler of the crawler system that determines which task should be scheduled the next time.

In the negative sampling part of the famous `word2vec`, the algorithm needs to randomly sample some negative words according to their frequencies in the corpus. [Codes link](https://github.com/tmikolov/word2vec/blob/master/word2vec.c#L527).

There are more examples in game developing: In games we often encounter random dropping of specified items by certain drop probability, such as falling silver coins 25%, gold coins 20%, diamonds 10%, equipment 5%, accessories 40%. The next dropped item type is now required to meet the above probability.

## Solutions

### Solution 1

The first method came up to me is to extend the uniform distributed random number generator. Let's begin with an example(all example programmes here after will be writen in Golang):
```go
var items   = []int{0, 1, 2, 3}
var weights = []float32{0.1, 0.3, 0.4, 0.2}
```
Making them conform to the weights what we’d do is something simple. Basically repeat the items 10x or even 100x times based on the numbers we have. So let’s say we’re repeating 10x times, this is the list we’ll end up with:
```go
var choices = []int{0, 1, 1, 1, 2, 2, 2, 2, 3, 3}
```
Then we can randomly choose a value from `choices`, and we are done. Full codes bellow:
```go
package main

import (
    "fmt"
    "math/rand"
)

func WeightedRandomS1(weights []float32) int {
    if len(weights) == 0 {
        return 0
    }
    var choices []int
    for i, w := range weights {
        wi := int(w * 10)
        for j := 0; j < wi; j++ {
            choices = append(choices, i)
        }
    }
    return choices[rand.Int()%len(choices)]
}

func main() {
    for i := 0; i < 100; i++ {
        fmt.Println(WeightedRandom([]float32{0.1, 0.3, 0.6}))
    }
}
```
In `word2vec`, this solution is adopted.

### Solution 2

The first solution takes too much memory, then came solution 2: Compute the discrete cumulative density function (CDF) of the list -- or in simple terms the array of cumulative sums of the weights. Then generate a random number in the range between 0 and the sum of all weights, do a linear search to find this random number in your discrete CDF array and get the value corresponding to this entry -- this is the weighted random number. Binary search could reduce the time complexity from $O(n)$ to $O(log(n))$.
```go
func WeightedRandomS2(weights []float32) int {
    if len(weights) == 0 {
        return 0
    }
    var sum float32 = 0.0
    for _, w := range weights {
        sum += w
    }
    r := rand.Float32() * sum
    for i, w := range weights {
        r -= w
        if r < 0 {
            return i
        }
    }
    return len(weights) - 1
}
```
I used this solution in the scheduler of [crawling framework](https://github.com/crawlerclub/x).

### Solution 3

The optimal solution for weighted random should be the [Alias Method](https://en.wikipedia.org/wiki/Alias_method). It requires $O(n)$ time to initialize, $O(1)$ time to make a selection, and $O(n)$ memory. A golang version implementation is [here](https://github.com/liuzl/alias).

#### Algorithm: Vose's Alias Method

##### Initialization:

1. Create arrays $Alias$ and $Prob$, each of size $n$.
2. Create two worklists, $Small$ and $Large$.
3. Multiply each probability by $n$.
4. For each scaled probability $p_i$:
   1. If $p_i<1$, add $i$ to $Small$.
   2. Otherwise $p_i \geqslant 1$, add $i$ to $Large$.
5. While $Small$ and $Large$ are not empty: ($Large$ might be emptied first)
   1. Remove the first element from $Small$; call it $l$.
   2. Remove the first element from $Large$; call it $g$.
   3. Set $Prob[l]=p_l$.
   4. Set $Alias[l]=g$.
   5. Set $p_g = p_g + p_l - 1$. (This is a more numerically stable option)
   6. If $p_g<1$, add $g$ to $Small$.
   7. Otherwise $p_g \geqslant 1$, add $g$ to $Large$.
6. While $Large$ is not empty:
   1. Remove the first element from $Large$; call it $g$.
   2. Set $Prob[g] = 1$.
7. While $Small$ is not empty: This is only possible due to numerical instability.
   1. Remove the first element from $Small$; call it $l$.
   2. Set $Prob[l] = 1$.

##### Generation:

1. Generate a fair die roll from an n-sided die; call the side $i$.
2. Flip a biased coin that comes up heads with probability $Prob[i]$.
3. If the coin comes up "heads", return $i$.
4. Otherwise, return $Alias[i]$.


## References
1. Walker, A. J. 1977. "An efficient method for generating discrete random variable with general distributions." *ACM Transactions on Mathematical Software* **3** 253–256.
2. "[Darts, Dice, and Coins: Sampling from a Discrete Distribution](http://www.keithschwarz.com/darts-dice-coins/)". *Keith Schwarz*, December 29, 2011
3. [Alias method](https://en.wikipedia.org/wiki/Alias_method). *Wikipedia*, April 5, 2018
4. [Weighted random selection from array
](https://stackoverflow.com/questions/4463561/weighted-random-selection-from-array), *stackoverflow*, Dec 16 2010
