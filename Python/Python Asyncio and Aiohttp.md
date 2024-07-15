This is a library for asynchronous programming, using [[Event Loops]] like in [[JavaScript]]. 
This is useful for I/O bound operations like network calls, reading/writing files, etc. where non-blocking behaviour can be useful.

When you **run asyncio**, you are creating an [[Event Loops]] that puts events (tasks) on the queue as they come up. When an asynchronous operation is finished, the callback gets placed on the queue.

This example isn't so useful, because we're just awaiting an operation:
```python
import asyncio

async def my_func():
		await asyncio.sleep(1)
		print("Func done")

async def main():
		await my_func()
		print("Main done")
		
asyncio.run(main())
```

Where it becomes super powerful is with **multiple, nested** awaits: here, we can await **several** tasks on the event loop, allowing any arbitrary number of async or sync tasks to run concurrently with non-blocking I/O. 
```python
import asyncio

async def task1():
		await asyncio.sleep(2)

def task2(): 
		for i in range(5):
				print(f"{i}")

async def main():
		await asyncio.gather(task1(), task2())

asyncio.run(main())
```

Chained awaits where an await depends on another async function with an await creates a 'promise' or 'callback'-like scenario. Once the first await is ready, the code after the higher-up await will execute.
### Network Calls
- We put N get requests onto the event queue
- They each send off their request and return an `awaitable`
- You then await all of the awaitables through `asyncio.gather()` - **this is a chained await**
- Then, you have access to all the results
```python
urls = []

async def get_request(session, url):
		async with session.get(url) as response:
				return await response.text()

async def main(): 
		async with aiohttp.ClientSession() as session:
				tasks = [get_request(session, url) for url in urls]
				results = await asyncio.gather(*tasks)

				for i, result in enumerate(results):
						print(f"Result {i} is {result[:100]}")

asyncio.run(main())
```