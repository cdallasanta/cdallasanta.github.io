---
layout: post
title:      "Benchmark, the New Standard <sup>tm</sup>"
date:       2019-10-01 17:52:41 +0000
permalink:  benchmark_the_new_standard_sup_tm_sup
---


A quick [Sentinels of the Multiverse](https://sentinelswiki.com/index.php?title=Benchmark) reference for the two fans out there.


When working on algorithms to practice for my upcoming technical interviews, I encountered an issue: How can I tell how fast my code runs, and if one is faster than the other? I could certainly figure out the [Big O notation](https://en.wikipedia.org/wiki/Big_O_notation) of my solutions, but what if they were very similar? Or if I wanted to compare my process to one of Ruby's methods? It turns out that Ruby has a built in tool for doing just that: `Benchmark`


### How to use it

At the top of your .rb file, add `require 'benchmark'`

To test a single method, call `Benchmark.measure` with a block like so:

```
puts Benchmark.measure {
  50000.times do
    # code to be tested here
  end
}
```

That will run the code to be tested 50,000 times, then print the time results to the console. Be careful if the code block or method you are running also uses `puts`, since it will do so 50,000 times. I learned that one the hard way by printing a large hash 50,000 times...


### Running comparisons

I wanted to use this for comparing several different methods of solving an algorithm though, and Benchmark comes with a very handy method: `#bm`, which is called like so:

```
n = 10000
testInput = [1,2,3,4]

Benchmark.bm do |benchmark|
  benchmark.report("Ruby Methods") do
    n.times do
      solvingWithRubyMethods(testInput)
    end
  end

  benchmark.report("My Methods") do
    n.times do
      solvingWithCustomMethods(testInput)
    end
  end
end
```

I set n to the number of times I want the test to run, and defined the input I wanted to test. Then, within the block, it will call each method n times using the defined input. When it finished running, I was presented with the following in the terminal:

```
              user       system     total        real
Ruby Methods  0.375000   0.000000   0.375000 (  0.373665)
My Methods    6.859375   0.000000   6.859375 (  6.890351)
```


So using ruby methods, the code ran 10,000 times in .37 seconds, compared to the manual version I wrote, which took 6.89 seconds to do the same. So, I certainly have some refactoring to do to make my code more efficient, but now I have a `Benchmark` I can use to test my improvements!
