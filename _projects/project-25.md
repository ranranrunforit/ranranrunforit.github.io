---
title: "Interview Preparations - Agent and MCP Cheatsheet 0"
excerpt: "When I have only 1 hour right before the interview...😅 Probably would need a day or two to review the whole series now <br/><img src='/images/Agent_Course_1.png'>"
collection: projects
date: 2025-06-29
category: LLM
description: "Notes from online materials."
tags:
  - LangChain
  - CrewAI
  - AutoGen
  - OpenAI
  - LangGraph
---

# Async Python

![image](/images/Agent_Course_2.png)
![image](/images/Agent_Course_3.png)
![image](/images/Agent_Course_4.png)
![image](/images/Agent_Course_5.png)

## A briefing on asynchronous python coding, essential in Agent engineering

This includes how to run async code from a python module.

# 📘 Async Python Tutorial

## Part 1: Introduction to Async – What, Why, and How?

### 🔄 What Is Async Python?

Async Python is a way to write code that doesn’t block. Instead of stopping everything while waiting for an operation to finish (like a web request, file read, or sleep), async Python lets other things run during the wait.

This is especially useful for **I/O-bound** operations — things that are slow because of external resources (like the internet or disk), not your CPU.

### 🤹 Async vs Threads vs Multiprocessing

| Feature      | `asyncio` (Async)      | Threads                | Multiprocessing      |
|--------------|----------------------|------------------------|---------------------|
| Use Case     | I/O-bound tasks      | I/O-bound (sometimes)  | CPU-bound tasks     |
| Concurrency  | Cooperative          | Pre-emptive            | True parallelism    |
| Overhead     | Low                  | Medium                 | High                |
| Complexity   | Medium               | Medium                 | High                |
| GIL Aware    | Yes                  | Yes                    | No                  |

- **Async** is single-threaded but can handle thousands of concurrent I/O tasks.
- **Threads** allow simultaneous operations but can have race conditions and are limited by the GIL (Global Interpreter Lock).
- **Multiprocessing** bypasses the GIL and runs in multiple processes, good for CPU-heavy tasks.

## Part 2: 🧠 The Event Loop

Think of the event loop as a conductor managing multiple instruments — tasks. It checks when a task is ready and then runs it, pausing it when it has to wait, and switching to another task.

```python
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)  # Pause here, switch to other tasks
    print("World")

asyncio.run(say_hello())
```

Behind the scenes:

- `say_hello()` is a coroutine.
- `asyncio.run()` starts an event loop.
- When `await asyncio.sleep(1)` is hit, the event loop pauses `say_hello` and can run other coroutines.

## Part 3: ✅ `async def` and `await`

### Basic Usage

```python
import asyncio

async def greet(name):
    print(f"Hello, {name}")
    await asyncio.sleep(1)
    print(f"Goodbye, {name}")

async def main():
    await greet("Alice")

asyncio.run(main())
```

### You must:

✅ Use `async def` to define coroutines

✅ Use `await` only inside `async def`

❌ Cannot use `await` at top-level (in scripts or modules) unless inside `asyncio.run()`

✅ Return values from async functions with `return`

## Part 4: Running Async Code from Cursor

✅ From a Python Module (e.g. `main.py`)

```python
# main.py
import asyncio

async def work():
    await asyncio.sleep(1)
    print("Async done!")

if __name__ == "__main__":
    asyncio.run(work())
```

✅ Just run `python main.py` in the terminal.

✅ From a Notebook (in Cursor or Jupyter)

Notebooks support top-level `await`!

```python
import asyncio

async def hello():
    await asyncio.sleep(1)
    print("Hello from notebook!")

await hello()  # works fine!
```

💡 You can also use `nest_asyncio` if you're embedding event loops (e.g., in some servers or LLM apps).

## Part 5: Async Code in Gradio Callbacks

Gradio lets you use `async def` directly for event handlers like button clicks.

```python
import gradio as gr
import asyncio

async def slow_response(name):
    await asyncio.sleep(2)
    return f"Hello, {name}! (after waiting)"

gr.Interface(fn=slow_response, inputs="text", outputs="text").launch()
```

🎉 It just works! Gradio uses `asyncio` under the hood.

## Part 6: Advanced Async – `asyncio.gather`

### Run multiple coroutines concurrently

```python
import asyncio

async def task(name, delay):
    await asyncio.sleep(delay)
    print(f"{name} done after {delay}s")
    return name

async def main():
    results = await asyncio.gather(
        task("A", 1),
        task("B", 2),
        task("C", 3)
    )
    print(results)

asyncio.run(main())
```

🧠 This runs all tasks *in parallel* (as far as I/O waits are concerned).

## Part 7: Tips, Traps, and Techniques

### ✅ Tips

- Use `asyncio.gather` to parallelize I/O-bound calls
- Use `async for` and `async with` for async iterators and context managers
- Use `anyio` or `trio` for higher-level async if needed

### ⚠️ Common Traps

| Trap                                 | Fix                              |
|---------------------------------------|----------------------------------|
| Trying to `await` at top-level in a script | Use `asyncio.run()`               |
| Mixing `threading` and `asyncio`          | Avoid or use with care           |
| Forgetting `await`                     | You’ll get a coroutine object and nothing runs |
| Blocking call (e.g., `time.sleep`)      | Use `await asyncio.sleep()` instead|

### 🔧 Debugging

Use:

```python
import asyncio
asyncio.get_running_loop().is_running()
```

…to verify if you're inside a running event loop.

## Part 8: Exercises to Try in a Notebook in Cursor

### Exercise 1 – Basic Coroutine

Write a coroutine that:

- Takes a name
- Waits 2 seconds
- Prints “Hello, [name]!”

### Exercise 2 – Parallel Coroutines

Write three async functions:

- `fetch_data()`
- `process_data()`
- `save_data()`

Each should wait 1 second and return a string. Use `asyncio.gather()` to run all three.

### Exercise 3 – Async Countdown

Write a coroutine that counts down from 5 to 1 with a 1-second wait in between each number.

### Exercise 4 – Compare Blocking vs Async

Run this and compare how long each takes:

```python
import time

def blocking():
    time.sleep(1)
    print("Blocking done")

async def non_blocking():
    await asyncio.sleep(1)
    print("Async done")
```

Create a version of both in a loop of 3 iterations. Time them using `time.time()` and compare.

### Exercise 5 – Build a Gradio async app

Create a Gradio app that:

- Takes a name
- Waits 2 seconds
- Returns a greeting

Try switching the handler between sync and async and measure the difference in responsiveness.

```python
# Exercise 1 – Basic Coroutine
import asyncio

async def greet(name):
    await asyncio.sleep(2)
    print(f"Hello, {name}!")

await greet("Alice")

# Exercise 2 – Parallel Coroutines
async def fetch_data():
    await asyncio.sleep(1)
    return "Data fetched"

async def process_data():
    await asyncio.sleep(1)
    return "Data processed"

async def save_data():
    await asyncio.sleep(1)
    return "Data saved"

results = await asyncio.gather(fetch_data(), process_data(), save_data())
print(results)

# Exercise 3 – Async Countdown
async def async_countdown():
    for i in range(5, 0, -1):
        print(i)
        await asyncio.sleep(1)
    print("Countdown complete!")

await async_countdown()

# Exercise 4 – Compare Blocking vs Async
import time

def blocking():
    time.sleep(1)
    print("Blocking done")

async def non_blocking():
    await asyncio.sleep(1)
    print("Async done")

# Blocking version
start = time.time()
for _ in range(3):
    blocking()
print("Blocking total:", round(time.time() - start, 2), "seconds")

# Async version
start = time.time()
await asyncio.gather(non_blocking(), non_blocking(), non_blocking())
print("Async total:", round(time.time() - start, 2), "seconds")

# Exercise 5 – Build a Gradio async app
import gradio as gr

async def async_greet(name):
    await asyncio.sleep(2)
    return f"Hello, {name}!"

def sync_greet(name):
    time.sleep(2)
    return f"Hello, {name}!"

# To use async handler:
gr.Interface(fn=async_greet, inputs="text", outputs="text", title="Async Greeting").launch()

# To use sync handler:
gr.Interface(fn=sync_greet, inputs="text", outputs="text", title="Sync Greeting").launch()
```

### And now some examples:

```python
# Let's define an async function

import asyncio

async def do_some_work():
    print("Starting work")
    await asyncio.sleep(1)
    print("Work complete")
```

```python
# What will this do?

do_some_work()
```

```python
# OK let's try that again!

await do_some_work()
```

```python
# What's wrong with this?

async def do_a_lot_of_work():
    do_some_work()
    do_some_work()
    do_some_work()

await do_a_lot_of_work()
```

```python
# Interesting warning! Let's fix it

async def do_a_lot_of_work():
    await do_some_work()
    await do_some_work()
    await do_some_work()

await do_a_lot_of_work()
```

```python
# And now let's do it in parallel
# It's important to recognize that this is not "multi-threading" in the way that you may be used to
# The asyncio library is running on a single thread, but it's using a loop to switch between tasks while one is waiting

async def do_a_lot_of_work_in_parallel():
    await asyncio.gather(do_some_work(), do_some_work(), do_some_work())

await do_a_lot_of_work_in_parallel()
```

### Finally - try writing a python module that calls do_a_lot_of_work_in_parallel

See the link at the top; you'll need something like this in your module:

```python
if __name__ == "__main__":
    asyncio.run(do_a_lot_of_work_in_parallel())
```

```python
import asyncio

async def do_some_work():
    print("Starting work")
    await asyncio.sleep(1)
    print("Work complete")

async def do_a_lot_of_work_in_parallel():
    await asyncio.gather(
        do_some_work(),
        do_some_work(),
        do_some_work()
    )

if __name__ == "__main__":
    asyncio.run(do_a_lot_of_work_in_parallel())
```


# Vibe coding and debugging

![image](/images/Agent_Course_10.png)


"Vibe coding" is the affectionate term for coding with the assistance of LLMs. It's particularly easy and wonderful to do with Cursor! But there are some good techniques to make it work well, which I cover on the course. Here are a few essential tips:

1. If you're prompting an LLM like ChatGPT or Claude to write code, include the current date in your prompt, and instruct the LLM to generate code that uses the latest versions of APIs. This is particularly important with the OpenAI API! ChatGPT frequently tries to use outdated versions of its own API...

2. Also in your prompts, ask LLMs to keep their code as short and simple as possible. LLMs seem to love to add lots of extra, unnecessary checks that clutters code and makes it hard to find problems.

3. Ask the same question to multiple LLMs, and pick the answer that is clearest and simplest.

4. Similar: take the answer from 1 LLM, and ask another LLM to verify it for correctness and whether it could be simpler or clearer.

5. Ask an LLM to give you 3 variations for ways to solve a problem, and pick the one that is clearest.

## Vibe Coding for a larger project

Try to avoid having an LLM generate 100-200 lines of code or more; it will be so hard for you to debug and figure out what's going wrong (unless you're already knowledgable).

Instead: start by breaking down your problem into small, independently testable steps, that are each relatively small. If you're not sure how to break down your problem - this is something you can ask the LLM to do!

Then for each of these building blocks:
- Use the tips above to have the LLM build the code
- Also have the LLM write tests to test and verify the code
- Test it yourself and satisfy yourself that it's working correctly

This will allow you to build a larger project with confidence.

## The golden rule: start small, work incrementally!



# Acknowledgements

This overview was created with the help of deep and manual research, drawing inspiration and information from the excellent resource:

[The Complete Agentic AI Engineering Course (2025)](https://www.udemy.com/course/the-complete-agentic-ai-engineering-course/)
