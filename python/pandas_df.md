# Pandas Dataframes

- `loc` vs. `iloc`: `loc` takes labels (stop index is **inclusive**), while `iloc` takes zero-based indices (stop index is **exclusive**). Both can be used to get or set data.

## Updating rows/columns
- Updating columns is the same as updating a dict where column labels are keys.
- Adding a new column as a linear combination of existing columns is supported:

```python
df['total'] = df['colA'] + df['colB']
df['avg'] = np.average(df, axis=1)
```

- Updating rows require methods such as `append()`, `concat()`, `drop()`, etc. Note that these methods do not modify data in-place by default and returns a new dataframe instead.
- When appending multiple rows, batch processing is more efficient (i.e, appending those rows to a list and then do a single concatenation).

## Sorting
- Sort by a column: `df.sort_values(by=<column label>, ascending=False)`
- Sort by multiple columns: `df.sort_values(by=[<colA>, <colB>], ascending=False)`. If values in `<colA>` are the same, order is resolved using the next column.
- Note that data are not updated in-place by default. New dataframes are returned.

## Filtering
Filtering takes two steps: (1) obtain a boolean array, and (2) apply the boolean array for filtering.

```python
high_avg_filter = df['avg'] > 90
high_avg_df = df[high_avg_filter]
```

### Filter and replace using `where()`
`where()` replace values where the condition is `False`:

```python
# Apply a lower bound (20) to the avg column
df = df.where(cond=df['avg'] > 20.0, other=20.0) 
```

## Statistics
- `df.describe()`: get basic statistics for each column
- Other specific statistics: `mean()`, `std()`, `median()`, etc.
- Missing data (represented as NaN) are skipped by default when computing statistics. They can be filled or interpolated with `fillna()` and `interpolate()`.


---
## References
[Pandas Dataframe](https://realpython.com/pandas-dataframe/)
