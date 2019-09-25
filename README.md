## Data 8 Cheat Sheet

![don't sue us pls](https://www.inferentialthinking.com/favicon.png)

Lost in the sea of documentation? Don't know where to put `np` or `are` or really anything? You've come to the right place!!!

This is a really condensed version of the docs that will keep updating throughout the semester to highlight the most crucial functions at any given time. 

Think of it as a happy medium between the [Python reference](http://data8.org/fa19/python-reference.html) and the [textbook](https://www.inferentialthinking.com/chapters/intro). At some point you'll probably want more info than the Python reference gives, but not quite at the level of the word-bombardment of the detailed textbook entries. 

#### PSA: Something missing/unclear? Contribute to the cheat sheet!!! [Edit the text directly](https://github.com/64bitpandas/data8-cheat-sheet/edit/master/README.md) or [Create an issue](https://github.com/64bitpandas/data8-cheat-sheet/issues)!

#### Also, this is very WIP right now. Check back in about a week or so and it'll be much more complete.

## Hot Functions
Stuff we just learned and is probably quite confuzzling at the moment.

**Joining two tables**

If something in Table1 Column is not found in Table2 Column, that row is left out of the joined table!
```python
# Match rows in table1 using values in 'Table1 Column' with rows in table2 using values in 'Table2 Column'.
table1.join('Table1 Column', table2, 'Table2 Column')

# If Table1 Column and Table2 Column have the same name, the call instead is
table1.join('Shared Column', table2)
```

**Plotting a table's column as a line**
```python
# plots the column as the x axis. Every column after it will be represented as lines in the line graph.
table.plot('column_name')

#can also plot by x,y. Makes a single line
table.plot('x_column','y_column')
```

In the example below, `table.plot('AGE')` was called on a `table` with three columns: `AGE`, `2010`, and `2014`.
![plot example](./img/plot-multiple.png)

**Plotting a table's column as a scatter plot**


## Table Operations

**Creating a table:**
```python
# both are equivalent
tbl = Table.read_table('tablename.csv')
tbl = Table().read_table('tablename.csv')
```

**Converting a table column into an array:**
```python
arr = tbl.column('column name')
```

**Creating a table column from an array:**
```python
# Remember to re-assign the original table if you want the new table to be saved!
tbl = tbl.with_column('column name', arr)

# Make as many columns as you want in a single function call!
tbl = tbl.with_columns('first column', arr1, 'second column', arr2, ... ,'nth column', arr_n)
```

**Getting specific rows or columns from a table**
```python
# ROW: Pass in either an array/list of 0-indexed row numbers, OR a single number.
first_row_only = tbl.take(0)
every_third_row = tbl.take([0, 3, 6...])

# COLUMN: Pass in either an array of column names, an array of column indices, OR individual names/indices.
first_col_only = tbl.column(0)
dank_memes_only = tbl.column(['Deep Fried Memes', 'Prequel Memes'])
```

**Sorting a Table**

Used to easily find max/min of a table, or check if a table has duplicate entries.
```python
# Default: starts at the lowest value and repeats are allowed. With descending=True, starts at the highest value instead. With distinct=False, repeats are ignored.
table.sort(column, descending=False, distinct=True)
```

## np aka "numpy"
**Creating an array which is a range of numbers**
```python
# Makes a range that goes from n1 to n2. Can optionally increment by n3. 
# n1, n2, n3 are all integers
arr_range = np.arange(n1,n2,n3=1)
```
**Calculating an average value / mean from a given array**
```python
np.average(array)
```
**Creating a new array that is the difference between sequential elements in a given array**
```python
#len of returned array is len(array)-1
# a=a-b
# b=b-c
# ...
np.diff(array)
```
**Creating a new array with elements that are in the form of (current element + all previous elements)**
```python
np.cumsum(array)
```

<!-- 
You can use the [editor on GitHub](https://github.com/64bitpandas/data8-cheat-sheet/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```python
def hi():
    a = b + 1
    return a
```

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/64bitpandas/data8-cheat-sheet/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.


**Bold** _italic_  -->