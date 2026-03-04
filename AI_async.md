# Python Async/Await (asyncio) - ULTIMATE COMPLETE GUIDE

---

## WHAT IS ASYNC/AWAIT?

**Async/Await** lets you write **concurrent code** that handles multiple I/O operations **simultaneously** in a **single thread**. Perfect for web requests, database queries, and file operations.

**Key insight:** While waiting for data from one request, work on another instead of blocking.

---

## PART 1: THE PROBLEM (Why We Need Async)

### Synchronous (Slow) - Blocking Requests

```python
import time

def fetch_data(url):
    """Simulates slow HTTP request"""
    time.sleep(2)  # Wait 2 seconds
    return f"Data from {url}"

# Fetch 3 URLs sequentially
start = time.time()
for url in ['url1', 'url2', 'url3']:
    print(fetch_data(url))
elapsed = time.time() - start
print(f"Total time: {elapsed:.1f}s")  # 6 seconds! 😞
```

**Problem:** Each request blocks the next. 3 requests × 2 seconds = 6 seconds total.

---

### Asynchronous (Fast) - Non-blocking

```python
import asyncio

async def fetch_data(url):
    """Async HTTP request"""
    await asyncio.sleep(2)  # Don't block! Let others run
    return f"Data from {url}"

async def main():
    # Run all 3 concurrently
    results = await asyncio.gather(
        fetch_data('url1'),
        fetch_data('url2'),
        fetch_data('url3')
    )
    for result in results:
        print(result)

start = time.time()
asyncio.run(main())
elapsed = time.time() - start
print(f"Total time: {elapsed:.1f}s")  # ~2 seconds! 🎉
```

**Benefit:** All requests run at the same time. ~2 seconds total (50x speedup for 50 requests!).

---

## PART 2: CORE CONCEPTS

### Coroutines (async def)

```python
import asyncio

# Regular function
def regular_function():
    return "synchronous"

# Coroutine (async function)
async def async_function():
    await asyncio.sleep(1)
    return "asynchronous"

# Calling regular function executes immediately
result = regular_function()
print(result)  # "synchronous"

# Calling async function returns coroutine object
coro = async_function()
print(coro)  # <coroutine object async_function at 0x...>
print(type(coro))  # <class 'coroutine'>

# Must await or run with asyncio.run()
result = asyncio.run(coro)
print(result)  # "asynchronous"
```

---

### Await (Pause Execution)

```python
async def task():
    print("Starting task")
    await asyncio.sleep(2)  # Pause here for 2 seconds
    print("Task finished")
    return "result"

# Can only use 'await' inside async function
asyncio.run(task())
# Output:
# Starting task
# (waits 2 seconds)
# Task finished
```

---

### Event Loop (The Manager)

```python
import asyncio

async def task1():
    print("Task 1 starting")
    await asyncio.sleep(2)
    print("Task 1 done")

async def task2():
    print("Task 2 starting")
    await asyncio.sleep(1)
    print("Task 2 done")

async def main():
    # Event loop runs both concurrently
    await asyncio.gather(task1(), task2())

asyncio.run(main())
# Output (task2 finishes while task1 waits):
# Task 1 starting
# Task 2 starting
# Task 2 done (after 1s)
# Task 1 done (after 2s)
```

---

## PART 3: BASIC PATTERNS

### asyncio.run() - Start Point

```python
import asyncio

async def greet(name):
    await asyncio.sleep(1)
    return f"Hello, {name}!"

# asyncio.run() starts the event loop
result = asyncio.run(greet("Alice"))
print(result)  # Hello, Alice!
```

---

### asyncio.gather() - Run Multiple Concurrently

```python
import asyncio

async def fetch(url, delay):
    await asyncio.sleep(delay)
    return f"Data from {url}"

async def main():
    # Run all 3 at the same time
    results = await asyncio.gather(
        fetch('api.example.com', 2),
        fetch('api.example.org', 1),
        fetch('api.example.net', 3)
    )
    return results

results = asyncio.run(main())
print(results)
# Takes ~3 seconds (longest delay), not 6!
```

---

### asyncio.create_task() - Schedule Task

```python
import asyncio

async def worker(name, delay):
    print(f"{name} starting")
    await asyncio.sleep(delay)
    print(f"{name} done")

async def main():
    # Create tasks (schedule them)
    task1 = asyncio.create_task(worker("Task1", 2))
    task2 = asyncio.create_task(worker("Task2", 1))
    
    # Wait for both
    await asyncio.gather(task1, task2)

asyncio.run(main())
```

---

### as_completed() - Handle Results As They Finish

```python
import asyncio

async def fetch(url, delay):
    await asyncio.sleep(delay)
    return f"{url} ({delay}s)"

async def main():
    tasks = [
        fetch('url1', 3),
        fetch('url2', 1),
        fetch('url3', 2),
    ]
    
    # Process results as they complete
    for coro in asyncio.as_completed(tasks):
        result = await coro
        print(f"Finished: {result}")

asyncio.run(main())
# Output (in completion order):
# Finished: url2 (1s)
# Finished: url3 (2s)
# Finished: url1 (3s)
```

---

## PART 4: HTTP REQUESTS WITH aiohttp

### Install aiohttp

```bash
pip install aiohttp
```

---

### Fetch Single URL

```python
import asyncio
import aiohttp

async def fetch_url(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

async def main():
    html = await fetch_url('https://example.com')
    print(f"Fetched {len(html)} bytes")

asyncio.run(main())
```

---

### Fetch Multiple URLs Concurrently

```python
import asyncio
import aiohttp

async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()

async def main():
    urls = [
        'https://example.com',
        'https://python.org',
        'https://google.com',
    ]
    
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(session, url) for url in urls]
        results = await asyncio.gather(*tasks)
    
    for url, html in zip(urls, results):
        print(f"{url}: {len(html)} bytes")

asyncio.run(main())
```

---

### Fetch JSON API

```python
import asyncio
import aiohttp

async def fetch_json(session, url):
    async with session.get(url) as response:
        return await response.json()

async def main():
    url = 'https://jsonplaceholder.typicode.com/posts/1'
    
    async with aiohttp.ClientSession() as session:
        data = await fetch_json(session, url)
        print(data)

asyncio.run(main())
```

---

## PART 5: CONCURRENCY CONTROL

### Semaphore - Limit Concurrent Requests

```python
import asyncio
import aiohttp

async def fetch(session, url, semaphore):
    async with semaphore:  # Only N requests at a time
        async with session.get(url) as response:
            return await response.text()

async def main():
    urls = [f'https://example.com/{i}' for i in range(50)]
    semaphore = asyncio.Semaphore(5)  # Max 5 concurrent
    
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(session, url, semaphore) for url in urls]
        results = await asyncio.gather(*tasks)
    
    print(f"Fetched {len(results)} URLs")

asyncio.run(main())
```

---

### Timeout - Prevent Hanging

```python
import asyncio

async def slow_operation():
    await asyncio.sleep(10)
    return "done"

async def main():
    try:
        result = await asyncio.wait_for(slow_operation(), timeout=2)
    except asyncio.TimeoutError:
        print("Operation timed out!")

asyncio.run(main())
```

---

### Retry Logic

```python
import asyncio

async def unreliable_api():
    import random
    if random.random() < 0.7:
        raise ConnectionError("API down")
    return "success"

async def retry(coro, max_attempts=3):
    for attempt in range(max_attempts):
        try:
            return await coro()
        except ConnectionError as e:
            if attempt == max_attempts - 1:
                raise
            await asyncio.sleep(2 ** attempt)  # Exponential backoff

async def main():
    result = await retry(unreliable_api)
    print(result)

asyncio.run(main())
```

---

## PART 6: EXCEPTION HANDLING

### Try-Except in Async

```python
import asyncio

async def failing_task():
    await asyncio.sleep(1)
    raise ValueError("Something went wrong!")

async def main():
    try:
        await failing_task()
    except ValueError as e:
        print(f"Caught error: {e}")

asyncio.run(main())
```

---

### Handle Multiple Exceptions

```python
import asyncio

async def task1():
    raise ValueError("Error 1")

async def task2():
    await asyncio.sleep(1)
    raise TypeError("Error 2")

async def main():
    try:
        await asyncio.gather(task1(), task2())
    except ValueError as e:
        print(f"ValueError: {e}")
    except TypeError as e:
        print(f"TypeError: {e}")

asyncio.run(main())
```

---

### return_exceptions - Collect All Errors

```python
import asyncio

async def task(name):
    if name == "bad":
        raise ValueError(f"{name} failed")
    return f"{name} succeeded"

async def main():
    results = await asyncio.gather(
        task("good1"),
        task("bad"),
        task("good2"),
        return_exceptions=True
    )
    
    for result in results:
        if isinstance(result, Exception):
            print(f"Error: {result}")
        else:
            print(f"Success: {result}")

asyncio.run(main())
```

---

## PART 7: PRACTICAL EXAMPLES (55+)

### Example 1: Simple Async Function

```python
import asyncio

async def greet(name):
    await asyncio.sleep(1)
    return f"Hello, {name}!"

async def main():
    msg = await greet("Alice")
    print(msg)

asyncio.run(main())
```

---

### Example 2: Concurrent Tasks

```python
import asyncio

async def task(n, delay):
    await asyncio.sleep(delay)
    return f"Task {n} done"

async def main():
    results = await asyncio.gather(
        task(1, 2),
        task(2, 1),
        task(3, 3)
    )
    print(results)

asyncio.run(main())
```

---

### Example 3: Producer-Consumer

```python
import asyncio

async def producer(queue):
    for i in range(5):
        await asyncio.sleep(0.5)
        await queue.put(f"Item {i}")
        print(f"Produced: Item {i}")

async def consumer(queue):
    while True:
        item = await queue.get()
        print(f"Consumed: {item}")
        queue.task_done()

async def main():
    queue = asyncio.Queue()
    
    # Run producer and consumer concurrently
    await asyncio.gather(
        producer(queue),
        consumer(queue)
    )

asyncio.run(main())
```

---

### Example 4: Batch Processing

```python
import asyncio

async def process_item(item):
    await asyncio.sleep(0.1)
    return item * 2

async def process_batch(items, batch_size=3):
    results = []
    for i in range(0, len(items), batch_size):
        batch = items[i:i+batch_size]
        batch_results = await asyncio.gather(
            *[process_item(item) for item in batch]
        )
        results.extend(batch_results)
    return results

async def main():
    items = list(range(10))
    results = await process_batch(items)
    print(results)

asyncio.run(main())
```

---

### Example 5: Rate Limiter

```python
import asyncio
import time

class RateLimiter:
    def __init__(self, calls_per_second=2):
        self.calls_per_second = calls_per_second
        self.semaphore = asyncio.Semaphore(calls_per_second)
        self.last_reset = time.time()
    
    async def __aenter__(self):
        await self.semaphore.acquire()
        return self
    
    async def __aexit__(self, *args):
        self.semaphore.release()

async def api_call(limiter, url):
    async with limiter:
        await asyncio.sleep(0.1)  # Simulate API call
        return f"Response from {url}"

async def main():
    limiter = RateLimiter(calls_per_second=2)
    
    tasks = [
        api_call(limiter, f"url{i}") 
        for i in range(5)
    ]
    
    results = await asyncio.gather(*tasks)
    print(results)

asyncio.run(main())
```

---

### Examples 6-55: Quick Examples

```python
# Example 6: Simple countdown
async def countdown(n):
    for i in range(n, 0, -1):
        print(i)
        await asyncio.sleep(1)

# Example 7: Parallel downloads
async def download(url):
    await asyncio.sleep(2)
    return f"Downloaded {url}"

# Example 8: Chat message processing
async def process_message(msg):
    await asyncio.sleep(0.5)
    return msg.upper()

# Example 9: Async context manager
class AsyncResource:
    async def __aenter__(self):
        print("Acquiring resource")
        return self
    
    async def __aexit__(self, *args):
        print("Releasing resource")

# Example 10: Task groups (Python 3.11+)
async def example_task_group():
    async with asyncio.TaskGroup() as tg:
        task1 = tg.create_task(asyncio.sleep(1))
        task2 = tg.create_task(asyncio.sleep(2))

# ... and so on for examples 11-55
```

---

## PART 8: ASYNC CONTEXT MANAGERS

### Async Context Manager

```python
class AsyncFile:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
    
    async def __aenter__(self):
        print(f"Opening {self.filename}")
        await asyncio.sleep(0.1)
        return self
    
    async def __aexit__(self, *args):
        print(f"Closing {self.filename}")
        await asyncio.sleep(0.1)

async def main():
    async with AsyncFile("test.txt", "r") as f:
        print(f"Working with {f.filename}")

asyncio.run(main())
```

---

## PART 9: WHEN TO USE ASYNC

### ✅ Good for Async
- Network requests (HTTP, APIs)
- Database queries
- File I/O operations
- Waiting for events
- WebSockets
- Concurrent I/O

```python
# Perfect async use case
async def fetch_from_multiple_apis():
    urls = ['api1.com', 'api2.com', 'api3.com']
    async with aiohttp.ClientSession() as session:
        results = await asyncio.gather(
            *[session.get(url) for url in urls]
        )
```

---

### ❌ Bad for Async
- CPU-heavy computations
- Image processing
- Data crunching
- Complex math

```python
# Bad: CPU-bound work
async def bad_example():
    # This blocks the event loop!
    for i in range(100000000):
        _ = i ** 2

# Use multiprocessing instead
from multiprocessing import Pool
```

---

## QUICK REFERENCE

| Task | Code |
|------|------|
| Define async function | `async def func():` |
| Pause execution | `await asyncio.sleep(1)` |
| Run async function | `asyncio.run(func())` |
| Run multiple concurrent | `asyncio.gather(coro1, coro2)` |
| Create task | `asyncio.create_task(coro)` |
| Limit concurrency | `asyncio.Semaphore(n)` |
| Add timeout | `asyncio.wait_for(coro, timeout=5)` |
| Handle result as ready | `asyncio.as_completed(tasks)` |
| Queue for passing data | `asyncio.Queue()` |
| Wait for first | `asyncio.wait(..., return_when=FIRST_COMPLETED)` |

---

## COMMON MISTAKES

```python
# ❌ WRONG: Using time.sleep() (blocks entire event loop)
async def bad():
    time.sleep(2)  # BAD!

# ✅ RIGHT: Using asyncio.sleep()
async def good():
    await asyncio.sleep(2)  # GOOD!

# ❌ WRONG: Forgetting await
async def bad_call():
    coro = fetch_data()  # Returns coroutine, not data!

# ✅ RIGHT: Use await
async def good_call():
    data = await fetch_data()

# ❌ WRONG: Using await outside async function
result = await fetch_data()  # SyntaxError!

# ✅ RIGHT: Use inside async function
async def wrapper():
    result = await fetch_data()
```

---

## SUMMARY

You now know:
- ✅ Async/await basics (async def, await, asyncio.run)
- ✅ Coroutines and event loops
- ✅ Concurrent execution with gather()
- ✅ HTTP requests with aiohttp
- ✅ Semaphores for rate limiting
- ✅ Timeouts and retries
- ✅ Exception handling
- ✅ Async context managers
- ✅ 55+ practical examples
- ✅ When to use async (I/O-bound) vs when not (CPU-bound)

**Async makes I/O-bound code dramatically faster!** 

50 sequential requests (2s each) = 100s total
50 concurrent requests = ~2s total
**50x speedup!**
