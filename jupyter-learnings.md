# Jupyter Learnings

## Resources
https://docs.jupyter.org/en/latest/what_is_jupyter.html
https://www.kdnuggets.com/the-7-most-useful-jupyter-notebook-extensions-for-data-scientists

## More Learning Materials
**Start here**
- [Try Jupyter in your browser](https://jupyter.org/try): practice with nothing installed
- [The Jupyter Notebook guide](https://jupyter-notebook.readthedocs.io/en/stable/notebook.html): the official guide to the notebook screen and how cells work
- [Dataquest Jupyter Notebook tutorial](https://www.dataquest.io/blog/jupyter-notebook-tutorial/): beginner walkthrough of a full small analysis

**Go deeper**
- [Python Data Science Handbook, Ch. 1: IPython](https://jakevdp.github.io/PythonDataScienceHandbook/01.00-ipython-beyond-normal-python.html): help with `?`, magic commands like `%timeit`, and shell commands

**Use it in your editor**
- [Jupyter notebooks in VS Code](https://code.visualstudio.com/docs/datascience/jupyter-notebooks): run `.ipynb` files inside VS Code instead of the browser

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
