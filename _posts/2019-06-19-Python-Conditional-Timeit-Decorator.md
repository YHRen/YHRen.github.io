---
layout: post
title: Python Conditional Timeit Decorator
published: true
date: 2019-06-19
tag: [python]
---

## Introduction
The conditional timeit decorator will provide a convient way to measure the time spent on individual functions. 
The behavior of the timer will depend on the verbosity flag such that: 

1. `python main.py` the program will run quitely
1. `python main.py -v` the program will report the progress
1. `python main.py -vv` the program will report the timing for individual functions it contains.


## Timeit Decorator
Python [decorator](https://realpython.com/primer-on-python-decorators/) changes the default behavior of the wrapped function. 

```python 
import time, functools
def timeit_if(func, theta):
    
    @functools.wraps(func)
    def time_func(*args, **kwargs):
        if not theta:
            res = func(*args, **kwargs)
        elif theta > 1:
            start_time = time.time()
            print("----- profiling <", \
                func.__name__, "> -----")
            res = func(*args, **kwargs)
            print("----- %s seconds -----" \
                % (time.time()-start_time))
        else:
            print("----- running <", \
                    func.__name__, "> -----")
            res = func(*args, **kwargs)
            print("--------------------------")
        return res

    return time_func
```
Let's save this file as `timeit_if.py`.

## Use the Timeit Decorator
Suppose in our `main.py`, several functions are performed sequentially.
```python
import time
def foo():
    print("foo")
    time.sleep(5)

def bar():
    print("bar")
    time.sleep(10);

if __name__ == "__main__":
    foo()
    bar()
```
We want to profile each function when `-vv` is passed, and display the function name without profiling when `-v` is passed.
To do so, we can "decorate" `foo()` and `bar()` using our `timeit_if` we just wrote.

```python
import time, argparse
from functools import partial
from timeit_if import timeit_if

parser = argparse.ArgumentParser()
parser.add_argument('-v','--verbosity', action="count")
args = parser.parse_args()
timeit = partial(timeit_if, theta=args.verbosity)

@timeit
def foo():
    print("foo")
    time.sleep(2)

@timeit
def bar():
    print("bar")
    time.sleep(3);

if __name__ == "__main__":
    foo()
    bar()
```
