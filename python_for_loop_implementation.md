# Python implementation of for loops

### synchronous for loop
```python
for element in sequence:
    print(element)
```

is implemented as
```python
iterator = sequence.__iter__()
while True:
    try:
        element = iterator.__next__()
        print(element)
    except StopIteration:
        break
```

because `sequence` is used to actually make a generator thusly
```python
gen_sequence = (element for element in sequence)
```

or more explicitly
```python
def gen_sequence(sequence, index: int=0):
    while True:
        try:
            yield sequence.__getitem__(index)
            index += 1
        except IndexError:
            break
```

### asynchronous for loop
```python
import asyncio

async for element in asequence:
    print(element)
```

is implemented as
```python
aiterator = asequence.__aiter__()
while True:
    try:
        element = await aiterator.__anext__()
        print(element)
    except StopAsyncIteration:
        break
```

where `asequence` is an asynchronous generator or iterator, e.g. representing a stream of data from a socket in case of network jobs. Bear in mind that an asynchronous generator is not a sequence of asyncio tasks, which are rather handled by
```python
coros = [asyncio.create_task(fun()) for fun in functions]
foo(stuff)
results = await asyncio.gather(*coros)
``` 
but that's another story!
