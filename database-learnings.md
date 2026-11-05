# Database Learnings: CSV into SQLite

## Why SQLite
- SQLite is a whole database stored in **one file** (for example `data/nsch.db`). There's no server to install or start.
- Python has it built in (`import sqlite3`), so there's nothing new to add to `requirements.txt`.
- Java comparison: `sqlite3.connect()` is like `DriverManager.getConnection("jdbc:sqlite:nsch.db")`, and pandas writes the `CREATE TABLE` and `INSERT`s for you.

## Steps (Python + pandas)

**1. Read the CSV into a DataFrame**
```python
import sqlite3
import pandas as pd

df = pd.read_csv("data/kids.csv")
df.head()                          # check the columns look right before saving
```

**2. Save it as a table**
```python
with sqlite3.connect("data/nsch.db") as conn:     # creates the file if it doesn't exist
    df.to_sql("children", conn, if_exists="replace", index=False)
```
- `"children"` is the table name.
- `if_exists="replace"` drops the old table and makes a new one. Use `"append"` to add rows, or `"fail"` (the default) to stop if the table already exists.
- `index=False` stops pandas from saving its row numbers as an extra column.

**3. Check that it worked**
```python
with sqlite3.connect("data/nsch.db") as conn:
    pd.read_sql("SELECT COUNT(*) AS n FROM children", conn)           # should match len(df)
    pd.read_sql("PRAGMA table_info(children)", conn)                  # column names and types
    pd.read_sql("SELECT state, COUNT(*) AS n FROM children GROUP BY state", conn)
```
`pd.read_sql` returns a DataFrame, so you can go straight back to pandas and NumPy.

## Big Files: Load in Chunks
If the CSV is too big for memory, read and save it a piece at a time, like a buffered reader in Java.
```python
with sqlite3.connect("data/nsch.db") as conn:
    for i, chunk in enumerate(pd.read_csv("data/big.csv", chunksize=50_000)):
        mode = "replace" if i == 0 else "append"   # new table on the first chunk, then add
        chunk.to_sql("children", conn, if_exists=mode, index=False)
```

## Shortcut: the `sqlite3` Terminal Tool
macOS already has it installed.
```bash
sqlite3 data/nsch.db ".import --csv data/kids.csv children"
sqlite3 data/nsch.db "SELECT COUNT(*) FROM children;"
```
**Watch out:** this saves every column as `TEXT`, even numbers. Then `weight` sorts as text (`"100" < "9"`). The pandas way above figures out `INTEGER` / `REAL` for you, so use it for this project.

## From the NSCH File
The survey comes as Stata (`.dta`), not CSV. You don't need a CSV in between:
```python
df = pd.read_stata("data/<nsch file>.dta", convert_categoricals=False)
with sqlite3.connect("data/nsch.db") as conn:
    df.to_sql("nsch_2024", conn, if_exists="replace", index=False)
```
If you do want a CSV, run `df.to_csv("data/nsch_2024.csv", index=False)`.

## Things to Watch Out For
- **Keep the `.db` file in `data/`** so `.gitignore` keeps it out of GitHub, just like the raw survey file.
- **Check the row count** after every import. If it doesn't match `len(df)`, something was skipped or appended twice.
- **`"replace"` deletes the old table.** Use it only when you mean to start over.
- **Spaces or odd characters in column names** make SQL awkward. Rename them first with `df.rename(columns=...)`.

## More Learning Materials
- [pandas `to_sql` docs](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_sql.html): every option for saving a DataFrame as a table
- [Python `sqlite3` docs](https://docs.python.org/3/library/sqlite3.html): the built-in module, includes a short tutorial
- [SQLite CLI docs](https://sqlite.org/cli.html): the `sqlite3` terminal tool, including `.import`
- [SQLBolt](https://sqlbolt.com/): short interactive lessons to practice `SELECT`, `WHERE` and `GROUP BY` in the browser
