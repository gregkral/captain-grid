# Captain Grid

Open a CSV, Parquet, Excel and scroll it straight away. Pick two files and see what changed between them, row by row and cell by cell, side by side.

## Pre-release Installation

Captain Grid is currently in pre-release and is not yet available on the VS Code Marketplace. 

Download the latest extension here:
https://github.com/gregkral/captain-grid/tags

And install it via the command line with:
```bash
code --install-extension captain-grid-0.1.0.vsix
```

## Fast

A Parquet file of 1 million rows and 50 columns opens almost instantly, and you can scroll at once to the middle or the very end. You don't wait for the file to load first, and you don't page through it a chunk at a time.
The comparison is also very quick but a tad slower than the viewer.

## View

Open any supported file with **Open with Captain Grid** from the Explorer or the editor tab.

- Scroll anywhere in the file at once, top to bottom.
- **Global row search**: Type a keyword and instantly get all rows that match this word in *any* cell.
- Filter rows by typing a filter on a specific column.
- Sort by one or more columns.
- Pin columns so they stay on screen while you scroll sideways.
- **Hide columns** to clean up your view and focus on what matters.
- Copy a selection and paste it straight into a spreadsheet.
- Choose Captain Grid's interactive viewer for polars or pandas DataFrame and Series outputs in a Jupyter notebook.

## Compare

Select two files in the Explorer and choose **Captain Grid: Compare Selected**, run **Captain Grid: New Comparison** and choose a file for each side, or use **Select for Compare** on one and **Compare with Selected** on the other. **Open Changes** on a tabular file under git opens a comparison too.

- **Rows match by key.** Captain Grid proposes a key column for you; set your own, or several, from a column's header.
- **Ignore columns.** Mark columns to exclude them from row matching; their differences are treated as unimportant rather than removed from the comparison.
- **Columns match by name**, so a column added or moved in one file doesn't throw the rest out of line.
- **You see only what changed.** By default, rows and columns with no difference are hidden, so you can focus on what's different. Show the whole file with one click.
- **Jump between differences** with Next and Previous Difference. The margins show where the differences are in the file.
- **Sort, filter and pin columns**, as in the viewer. Click a column's difference count to see only the rows that differ in that column.
- **Ignore what doesn't matter**: padding, capitalisation, the same date written two ways, an empty cell against a missing one, small numeric differences. Open Captain Grid Settings to turn these rules on or off. The differences they cover are still marked so you know they're there.
- `007` and `7` are the same value: numbers compare as numbers, whatever their text.

## Supported files

| Format | Extensions |
| --- | --- |
| Delimited text | `.csv`, `.tsv` |
| Excel and spreadsheets | `.xlsx`, `.xlsm`, `.xlsb`, `.xls`, `.ods` |
| Parquet | `.parquet` |
| Arrow / Feather | `.arrow`, `.feather`, `.ipc` |
| JSON lines | `.ndjson`, `.jsonl` |

When Captain Grid guesses a delimited file's delimiter or text encoding wrong, change it from **Read as** on the tab's bar. **Other…** takes any one-character delimiter or any text encoding Python knows.

## You don't need Python

Captain Grid runs on polars, which is a Python library, but you don't need Python on your machine. The first time you use Captain Grid it downloads its own Python environment with [uv](https://docs.astral.sh/uv/), separate from any Python you already have. That first run needs an internet connection (about 90 MB) and takes from about 10 seconds to a few minutes. It happens once.

If something goes wrong, **Captain Grid: Show Engine Log** shows what the engine said. If the environment gets into a bad state, run **Captain Grid: Rebuild Python Environment**.

Captain Grid needs VS Code 1.90 or later, on Windows, macOS or Linux, x64 or Arm.
Note: It has only been tested on Mac and Windows.

## Known limitations

- **Workbook error cells read as empty.** `#N/A`, `#DIV/0!` and other errors read as an empty cell, so two different errors compare as the same.
- **Workbook duration cells read as dates.** A time like `255:10:10`, formatted `[hh]:mm:ss`, reads as `1900-01-10 15:10:10`, or as `1904-01-11 15:10:10` in a workbook saved in the 1904 date system. Two workbooks that differ only in date system then show that cell as changed.
- **Workbook numbers are rounded to nine decimal places by default.** Turn on `captainGrid.exactWorkbookNumbers` to read numbers at full precision; opening workbooks is about 60% slower. To report differences smaller than the default `captainGrid.epsilon` of `0.00001`, lower epsilon too.
- **The first cell run in a new notebook kernel isn't drawn by Captain Grid.** The display hook is installed after Captain Grid first sees the kernel, so rerun that cell to get the inline viewer. Later runs, including after a kernel restart, are drawn. For a cell ending in a dataframe variable, **Open 'df' with Captain Grid** on its status bar opens a viewer tab instead.
