---
{"dg-publish":true,"permalink":"/020-programming/python-pivot-tables/"}
---

---

1. **Pivot Table Syntax**: Pivot tables in pandas require the entire DataFrame as the starting point, not just a single column. The basic syntax is `df.pivot_table()`.
2. **Parameters Structure**:
    - `values`: Specifies which column(s) to aggregate (like "avg_temp_c")
    - `index`: Defines the row labels (can be a single column name or a list of columns)
    - `columns`: Defines the column labels (typically a categorical variable like "year")
    - `aggfunc`: Defines how to aggregate (default is mean, but can be sum, count, etc.)
3. **Multi-level Indexing**: When using multiple columns for the index parameter (like `["country", "city"]`), you create a hierarchical index, not a tuple.
4. **Data Type Requirements**: For the columns parameter to work properly, the data needs to be in the correct type (e.g., years should be extracted from dates).
5. **Default Aggregation**: If you don't specify an aggregation function, pivot_table uses mean by default, which is often what you want for temperature data.
6. **Result Structure**: The resulting pivot table will have rows for each unique combination of index values and columns for each unique value in the column parameter.

Understanding these concepts will help you create more effective pivot tables for data analysis and visualization.


## What Went Wrong

1. **Incorrect tuple formatting in the second line**:

```python
temp_by_country_city_vs_year.loc[("Egypt"):("Cairo","India"):("Dheli")]
```

    - You used three tuples with two colons, which is invalid syntax
    - The tuple structures were inconsistent (first had only country, others had mixed elements)
    - You misspelled "Delhi" as "Dheli"
    - You can't chain slices with multiple colons like this
2. **Improper hierarchy in the third line**:

```python
temp_by_country_city_vs_year.loc[("Egypt"):("Cairo","Dheli"),"2005":"2010"]
```

    - The start tuple had only country, but the end tuple had both country and city
    - Year values were in quotes when they should be numeric (if stored as integers)
    - Again, "Delhi" was misspelled

## How to Avoid These Mistakes

1. **Understand the structure of your multi-index**:
    - Check your index structure with `temp_by_country_city_vs_year.index` before slicing
    - Each tuple in a slice must have the same number of elements as levels in your index
2. **Use consistent tuple structures**:
    - For a slice like `('Egypt', 'Cairo'):('India', 'Delhi')`, both tuples must have the same structure
    - The elements must appear in the same order as your index levels (country first, then city)
3. **Match data types in your slices**:
    - If your columns are numeric years (like `2005`), don't use strings like `"2005"` in slices
    - If they are strings, use strings in your slice
4. **Visualize the hierarchy**:
    - Think of multi-indices as nested dictionaries - you need to specify each level
    - Country → City is a hierarchy, not independent elements
5. **Use proper pandas slice syntax**:
    - Always use one colon per dimension: `start:end` for rows, and separate with a comma for columns
    - For example: `.loc[row_start:row_end, col_start:col_end]`
6. **Test with smaller examples**:
    - If unsure, try selecting a single element first (e.g., `.loc[('Egypt', 'Cairo'), 2005]`)
    - Then expand to slices once you confirm the structure works
7. **Use .loc's alternative syntax for complex cases**:
    - For very complex selections, consider using `.xs()` or breaking down into multiple steps

Remember that the tuple structure must precisely match your index hierarchy, with the correct number of elements in the right order.

