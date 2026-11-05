# Jupyter Learnings

## Resources
https://docs.jupyter.org/en/latest/what_is_jupyter.html
https://www.kdnuggets.com/the-7-most-useful-jupyter-notebook-extensions-for-data-scientists

## More Learning Materials
**Start here**
- [Try Jupyter in your browser](https://jupyter.org/try): practice with nothing installed
- [The Jupyter Notebook guide](https://jupyter-notebook.readthedocs.io/en/stable/notebook.html): the official guide to the notebook screen and how cells work
- [Dataquest Jupyter Notebook tutorial](https://www.dataquest.io/blog/jupyter-notebook-tutorial/): beginner walkthrough of a full small analysis

- [Real Python: Jupyter Notebook introduction](https://realpython.com/jupyter-notebook-introduction/): installing, running cells, and exporting a notebook, step by step

**Go deeper**
- [Python Data Science Handbook, Ch. 1: IPython](https://jakevdp.github.io/PythonDataScienceHandbook/01.00-ipython-beyond-normal-python.html): help with `?`, magic commands like `%timeit`, and shell commands
- [IPython built-in magic commands](https://ipython.readthedocs.io/en/stable/interactive/magics.html): the full list of `%` and `%%` commands, use it as a reference
- [JupyterLab docs](https://jupyterlab.readthedocs.io/en/stable/): the newer Jupyter screen with tabs and a file browser, a bit closer to an IDE

**Writing notes in Markdown cells**
- [Markdown Guide: basic syntax](https://www.markdownguide.org/basic-syntax/): headings, lists, bold, and links for your note cells

**Use it in your editor**
- [Jupyter notebooks in VS Code](https://code.visualstudio.com/docs/datascience/jupyter-notebooks): run `.ipynb` files inside VS Code instead of the browser
- [Google Colab](https://colab.research.google.com/): free notebooks in the cloud, handy for trying something without touching your `venv`

## What a Notebook Is
- A notebook (`.ipynb`) is a list of **cells**. Each cell is either **code** or **Markdown** (notes).
- You run one cell at a time and see its output right under it. In Java you'd rerun the whole `main()`. Here you rerun only the part you changed.
- Variables stay in memory between cells, like fields on one long-running object.

## Starting It
```bash
source venv/bin/activate   # always activate the project environment first
jupyter notebook           # opens in your browser
```
Stop it with `Ctrl + C` in the terminal.

## Keyboard Shortcuts
| Keys | What it does |
|---|---|
| `Shift + Enter` | Run the cell and move to the next one |
| `Esc` | Command mode (to use the keys below) |
| `Enter` | Edit mode (to type in a cell) |
| `A` / `B` | Add a cell above / below |
| `D D` | Delete the cell |
| `M` / `Y` | Turn a cell into Markdown / code |

## Useful Tricks
- The last line of a cell prints itself. `df.head()` alone shows a table, no `print()` needed.
- `df.` + `Tab` shows the available methods, like autocomplete in an IDE.
- `df.head?` shows the help for a method.

## Things to Watch Out For
- **Run order matters.** The `[3]` next to a cell shows the order it ran in, not where it sits on the page. If you run cells out of order, variables can hold old values.
- **When in doubt, restart.** Use **Kernel → Restart & Run All** to run everything from the top. If that works, the notebook is correct.
- **Relative paths start from the notebook's folder.** From `notebooks/`, the data is at `../data/`.

## Magic Commands
Lines that start with `%` are Jupyter commands, not Python.
```python
%timeit np.average(x, weights=w)   # runs the line many times and reports how fast it is
%who                               # lists the variables in memory right now
%pwd                               # shows the current folder (handy for path problems)
!pip list                          # "!" runs a terminal command from the cell
```
Make sure `!pip install` goes into the project `venv`. If unsure, install from the terminal instead.

## Charts in a Notebook
```python
import matplotlib.pyplot as plt
plt.bar(["0-5", "6-11", "12-17"], [1.2, 3.1, 3.8])
plt.show()                         # the chart appears under the cell
```
Draw one chart per cell, so each one sits next to the note that explains it.

## Saving Notebooks to Git
- A notebook file stores its **outputs** (tables, charts) along with the code. That makes diffs long and messy.
- Before committing, use **Kernel → Restart & Run All**, check it works, then decide: keep outputs (GitHub shows the charts) or clear them with **Edit → Clear Outputs of All Cells** (smaller diffs).
