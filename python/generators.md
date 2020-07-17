# Generators and Data Pipelines

Generators are useful for building data pipelines (i.e., producer-processing-consumer chains). Using generators can reduce memory footprint because next items are fetched on demand. The downside, however, is potential increase of latency.

## General Form

```python

def producer():
    ...
    yield ...

def processing(gen_obj):
    for x in gen_obj:
        yield process(x)

def consumer(gen_obj):
    for x in gen_obj:
        use(x)

## app code
gen_obj = producer()
gen_obj = processing(gen_obj)
consumer(gen_obj)

```

`producer()` and `processing()` are generators because they contain `yield`. They return a generator object when called. Note it is the for-loop in `consumer()` which implicitly calls `.__next__()` that triggers execution in `producer()` and `processing()`.


## Example: real-time log processing

```python

def tail(filename):
    f = open(filename)
    f.seek(0, os.SEEK_END)
    while True:
        line = f.readline()
        if not line:
            time.sleep(0.1)
            continue
        yield line

lines = tail('log.csv')
row = csv.reader(lines)
rows = select_columns(rows, [0, 1, 4])
rows = convert_types(rows, [str, float, float])
rows = make_dicts(rows, ['name', 'price', 'change'])
for row in rows:
	print(row)       

```

## Generator expressions

The syntax of generator expressions is similar to list comprehension, but a generator object is created instead. When using built-in reduction functions, consider generator expressions in addition to list comprehensions.

For example, the second usage is preferable than the firs one for a long list:

```python
# 1
sum([x**2 for x in longlist]) 
# 2
sum(x**2 for x in longlist)
```

Another usage is to replace short generators:
```python
def make_dict(rows, headers):
    for row in rows:
        yield dict(zip(headers, row))

## equals

def make_dict(rows, headers):
    return (dict(zip(headers, row)) for row in rows)
```

---
## References
[Practical Python (Chapter 6)](https://github.com/dabeaz-course/practical-python/blob/master/Notes/06_Generators/00_Overview.md)
