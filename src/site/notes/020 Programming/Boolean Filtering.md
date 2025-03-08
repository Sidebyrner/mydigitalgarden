---
{"dg-publish":true,"permalink":"/020-programming/boolean-filtering/"}
---

---

## Understanding Boolean Filtering for Min/Max Values

In the context of finding extreme values (like the city with the lowest temperature), your main challenges were:

### Where You Went Wrong

1. **Incorrect axis selection**: You used `axis=0` for city temperatures, which calculates means across rows (giving yearly averages) instead of across columns (giving city averages).
2. **Non-Boolean approach**: In your attempt to find the lowest temperature city, you tried to use `.mean(axis=2)` on a subset instead of using Boolean filtering.
3. **Incorrect column access**: The expression `mean_temp_by_city['city']` would fail because after calculating means, you have a Series with index labels, not a DataFrame with column names.

### Key Learnings for Future Cases

4. **Axis understanding is critical**:
    - `axis=0` operates on rows (down columns)
    - `axis=1` operates across columns (across rows)
    - Visualize your data structure before choosing the axis
5. **Boolean filtering pattern for extremes**:

```python
# Pattern to find extreme values:
data[data == data.min()]  # For minimums
data[data == data.max()]  # For maximums
```

6. **Alternative methods with trade-offs**:
    - `idxmin()` and `idxmax()` give you only the index labels
    - Boolean filtering gives you both indices and values
    - Choose based on what information you need
7. **Series vs. DataFrame awareness**:
    - After aggregation (like `.mean()`), check if your result is a Series or DataFrame
    - Series use `[boolean_mask]` for filtering
    - DataFrames can use `.loc[boolean_mask]` for more clarity
8. **Test your understanding**: When uncertain, try examining your data structure at each step:

```python
print(type(mean_temp_by_city))
print(mean_temp_by_city.head())
```


The most fundamental concept is that Boolean filtering creates a mask of True/False values matching your original data structure, keeping only values where the condition is True.

