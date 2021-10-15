---
title: 'Advent of code 2020'
permalink: /posts/2020/12/25-advent-of-code
date: 2020-12-25
tags:
  - code
  - puzzles
  - python
---

Last year I spent some free time working on the [Advent of Code](https://adventofcode.com/2020) puzzles. I mainly used pure Python, probably with some NumPy sprinkled in.

[Click here to view my solutions on Github](https://github.com/jasnyder/advent2020)


# Day 15
**WARNING: The following contains spoilers for how I solved Day 15 of the Advent of Code 2020!!**
The puzzle for Day 15 I found especially interesting. The statement of the puzzle is rather simple: it defines a sequence, gives the first few terms, and asks for the 2020-th term.

## Setup
The sequence is defined as follows:
```
  In this game, the players take turns saying numbers. They begin by taking turns reading from a list of starting numbers (your puzzle input). Then, each turn consists of considering the most recently spoken number:

  If that was the first time the number has been spoken, the current player says 0.
  Otherwise, the number had been spoken before; the current player announces how many turns apart the number is from when it was previously spoken.
  So, after the starting numbers, each turn results in that player speaking aloud either 0 (if the last number is new) or an age (if the last number is a repeat).

  For example, suppose the starting numbers are 0,3,6:

  Turn 1: The 1st number spoken is a starting number, 0.
  Turn 2: The 2nd number spoken is a starting number, 3.
  Turn 3: The 3rd number spoken is a starting number, 6.
  Turn 4: Now, consider the last number spoken, 6. Since that was the first time the number had been spoken, the 4th number spoken is 0.
  Turn 5: Next, again consider the last number spoken, 0. Since it had been spoken before, the next number to speak is the difference between the turn number when it was last spoken (the previous turn, 4) and the turn number of the time it was most recently spoken before then (turn 1). Thus, the 5th number spoken is 4 - 1, 3.
  Turn 6: The last number spoken, 3 had also been spoken before, most recently on turns 5 and 2. So, the 6th number spoken is 5 - 2, 3.
  Turn 7: Since 3 was just spoken twice in a row, and the last two turns are 1 turn apart, the 7th number spoken is 1.
  Turn 8: Since 1 is new, the 8th number spoken is 0.
  Turn 9: 0 was last spoken on turns 8 and 4, so the 9th number spoken is the difference between them, 4.
  Turn 10: 4 is new, so the 10th number spoken is 0.
```

The question is: what number is said on the 2020th turn?

## Part 1 solution
As given, the rules are not too hard to implement in code. Here's how I did it:

First, read in the input (the first 6 numbers spoken) and put them into a list, which I'm calling `nums`.
```python
with open('input.txt','r') as fobj:
    raw = fobj.read().split(',')
    nums = [int(n) for n in raw]
```

My code will modify the list `nums` until it has length 2020, and then will stop and output the last number that was added. To figure out which number to add next, I need to:
 * look at the most recent number
 * figure out if that number was said previously to the last turn
 * * if not, output zero
 * * if so, find the most recent occurrence and output how many turns ago it was

 Because I want to make the code as simple as possible (i.e. use only builtin python functions whenever possible), I'll use the `.index()` method of `list` objects to find prior occurrences. Since `.index()` finds the *first* occurrence of the desired element, I'll keep the list of numbers in reverse order:
 ```python
nums.reverse()
 ```

Now to update the list until it's the right length! This is fairly straightforward:
 ```python
 while len(nums)<2020:
    last_spoken = nums[0]
    try:
        i = nums[1:].index(last_spoken)
        next_spoken = i + 1
    except ValueError:
        next_spoken = 0
    
    nums.insert(0, next_spoken)

with open('output1.txt','w') as fobj:
    fobj.write(str(nums[0]))
```

## Part 2
Every Advent of Code puzzle has two parts, with the second being somehow more involved or difficult than the first. This day's Part 2 is somewhat unique is that it introduces no further mechanics or extra data, but just asks for a *much* later entry in the sequence: the 30000000th (that's 30 million).

My first approach was to simply use the same code as for Part 1, but with 30000000 in place of 2020. As you can probably guess, that didn't work. The time complexity of that algorithm is quadratic, since to compute the `t+1`-st entry of the sequence, you have to search through the entire existing sequence of length `t`. Assuming that on average you'll need to go `t/2` steps back to find the number you need, the number of operations needed to compute `T` terms in total is on the order `T**2 / 4`. Not good!

In practice, I was able to compute 30000 terms in about 200 seconds on my laptop. Assuming the quadratic scaling holds, that would mean getting out to 30000000 would take about 6.5 years!

Clearly, getting out to the 30-millionth term in this sequence requires a different approach. Specifically, we need to eliminate the need to do a list search at every step. To do that, I decided to keep track of only the strictly-required information in a dictionary. That information is: which numbers have been said before, and when.

To make a turn and update the dictionary, I will look at the most-recently-said number, find out if it's been said previously to the most recent turn, and output the appropriate number based on the result.

My implementation looks like this; first, read in the data
```python
with open('input.txt','r') as fobj:
    raw = fobj.read().split(',')
    nums = [int(n) for n in raw]
```

Next, initialize the dictionary `last_spoken` with the data provided. I leave out the last input term so I can use it as the "current" number
```python
last_spoken = dict()
for turn, num in enumerate(nums[:-1]):
    last_spoken[num] = turn
```

Now we define a routine called `next` that takes in the dictionary, together with the current number and the current turn. This function does two things: updates the `last_spoken` dictionary, and computes the next number. Importantly, it computes the next number *before* updating the dictionary. If the dictionary were updated first, there would be no way to tell whether or not the current number has been spoken preveiously to the most recent turn (at least without storing additional information...)
```python
def next(current_num, last_spoken, turn):
    if current_num in last_spoken.keys():
        next_num = turn - last_spoken[current_num]
    else:
        next_num = 0
    last_spoken[current_num] = turn
    return next_num
```

Finally, all that's left to do is set `current_num` and `turn`, and compute until we reach the desired term!
```python
current_num = nums[-1]
turn = len(nums) - 1

while turn < 30000000 - 1:
    current_num = next(current_num, last_spoken, turn)
    turn += 1

with open('output2.txt','w') as fobj:
    fobj.write(str(current_num))
```

Bonus! You can actually rewrite the `next` function in a simpler way, using the `.get()` method of `dict` objects. The `.get()` works just like accessing with a key, but you can choose a default value to be returned if the key is not found. This abstracts away the `if` statement in the above implementation: the default value of `turn` makes it so that `next_num = 0` in the case that `current_num` is not in `last_spoken.keys()`.
```python
def next(current_num, last_spoken, turn):
    next_num = turn - last_spoken.get(current_num, turn)
    last_spoken[current_num] = turn
    return next_num
```

# Conclusion
This was a cool problem