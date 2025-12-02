---
tags:
  - python
  - pandas
  - tips
---
# Pandas tips and gotchas

### Create new columns with all null values

```python
# Resulting dtype: object (string)
df[["A", "B"] = None

# Resulting dtype: float64
df[["A", "B"] = np.nan
```

### Assign value(s) to columns of filtered rows

```python
# First create a boolean mask to filter for specific rows.
mask = (df["A"] == "value") & (df["B"] == "value")

# Filter for specific rows and columns and assign values to all.
df.loc[mask, ["C", "D"]] = "another_value", "another_value"
```

### Gotcha: Chaining assignment

Don't do this:
```python
df["A"]["B"] = "value"
```

- `df["A"]["B"]` is called "chained indexing".
- Assigning values this way is called "chained assignment". Raises a `SettingWithCopyWarning`.
- Can lead to unexpected behaviour because each indexing operation may return either a copy or a view of the original data.
- Assigning to a copy leads to data loss as the copy is discarded immediately post operation.
- Use `.loc` or `.iloc` instead - guarantees assignment to original data frame.