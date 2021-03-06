# wasabi: A lightweight console printing and formatting toolkit

Over the years, I've written countless implementations of coloring and
formatting utilities to output messages in our libraries like
[spaCy](https://spacy.io), [Thinc](https://github.com/explosion/thinc) and
[Prodigy](https://prodi.gy). While there are many other great open-source
options, I've always ended up wanting something slightly different or slightly
custom.

This package is still a work in progress and aims to bundle those utilities in
a standardised way so they can be shared across our other projects. It's super
lightweight, has zero dependencies and works across Python 2 and 3.

[![Travis](https://img.shields.io/travis/ines/wasabi/master.svg?style=flat-square&logo=travis)](https://travis-ci.org/ines/wasabi)
[![Appveyor](https://img.shields.io/appveyor/ci/inesmontani/wasabi/master.svg?style=flat-square&logo=appveyor)](https://ci.appveyor.com/project/inesmontani/wasabi)
[![PyPi](https://img.shields.io/pypi/v/wasabi.svg?style=flat-square)](https://pypi.python.org/pypi/wasabi)
[![GitHub](https://img.shields.io/github/release/ines/wasabi/all.svg?style=flat-square)](https://github.com/ines/wasabi)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg?style=flat-square)](https://github.com/ambv/black)

<img width="609" src="https://user-images.githubusercontent.com/13643239/48663861-8c9ea000-ea96-11e8-8b04-d120c52276a8.png">

## 💬 FAQ

### Are you going to add more features?

Yes, there's still a few of helpers and features to port over. However, the new
features will be heavily biased by what we (think we) need. I always appreciate
pull requests to improve the existing functionality – but I want to keep this
library as simple, lightweight and specific as possible.

### Can I use this for my projects?

Sure, if you like it, feel free to adopt it! Just keep in mind that the package
is very specific and not intended to be a full-featured and fully customisable
formatting library. If that's what you're looking for, you might want to try
other packages – for example, [`colored`](https://pypi.org/project/colored/),
[`crayons`](https://github.com/kennethreitz/crayons),
[`colorful`](https://github.com/timofurrer/colorful),
[`tabulate`](https://bitbucket.org/astanin/python-tabulate),
[`console`](https://github.com/mixmastamyk/console) or
[`py-term`](https://github.com/gravmatt/py-term), to name a few.

### Why `wasabi`?

I was looking for a short and descriptive name, but everything was already taken.
So I ended up naming this package after one of my rats, Wasabi. 🐀

## ⌛️ Installation

```bash
pip install wasabi
```

## 🎛 API

### <kbd>class</kbd> `Printer`

#### <kbd>method</kbd> `Printer.__init__`

```python
from wasabi import Printer

msg = Printer()
```

| Argument | Type | Description | Default |
| --- | --- | --- | -- |
| `pretty` | bool | Pretty-print output with colors and icons. | `True` |
| `no_print` | bool | Don't actually print, just return. | `False` |
| `colors` | dict | Add or overwrite color values, names mapped to `0`-`256`. | `None` |
| `icons` | dict | Add or overwrite icon. Name mapped to unicode. | `None` |
| `line_max` | int | Maximum line length (for divider). | `80` |
| `animation` | unicode | Steps of loading animation for `Printer.loading`. | `"⠙⠹⠸⠼⠴⠦⠧⠇⠏"` |
| `animation_ascii` | unicode | Alternative animation for ASCII terminals. | `"\|/-\\"` |
| `hide_animation` | bool | Don't display animation, e.g. for logs. | `False` |
| `ignore_warnings` | bool | Don't output messages of type `MESSAGE.WARN`. | `False` |
| **RETURNS** | `Printer` | The initialized printer. | - |

#### <kbd>method</kbd> `Printer.text`

```python
msg = Printer()
msg.text("Hello world!")
```

| Argument | Type | Description | Default |
| --- | --- | --- | -- |
| `title` | unicode | The main text to print. | `""` |
| `text` | unicode | Optional additional text to print. | `""` |
| `color` | unicode / int | Color name or value. | `None` |
| `icon` | unicode | Name of icon to add. | `None` |
| `show` | bool | Whether to print or not. Can be used to only output messages under certain condition, e.g. if `--verbose` flag is set. | `True` |
| `no_print` | bool | Don't actually print, just return. Overwrites global setting. | `False` |
| `exits` | int | If set, perform a system exit with the given code after printing. | `None` |

#### <kbd>method</kbd> `Printer.good`, `Printer.fail`, `Printer.warn`, `Printer.info`

Print special formatted messages.

```python
msg = Printer()
msg.good("Success")
msg.fail("Error")
msg.warn("Warning")
msg.info("Info")
```

| Argument | Type | Description | Default |
| --- | --- | --- | -- |
| `title` | unicode | The main text to print. | `""` |
| `text` | unicode | Optional additional text to print. | `""` |
| `show` | bool | Whether to print or not. Can be used to only output messages under certain condition, e.g. if `--verbose` flag is set. | `True` |
| `exits` | int | If set, perform a system exit with the given code after printing. | `None` |

#### <kbd>method</kbd> `Printer.divider`

Print a formatted divider.

```python
msg = Printer()
msg.divider("Heading")
```
| Argument | Type | Description | Default |
| --- | --- | --- | -- |
| `text` | unicode | Headline text. If empty, only the line is printed. | `""` |
| `char` | unicode | Single line character to repeat. | `"="` |
| `show` | bool | Whether to print or not. Can be used to only output messages under certain condition, e.g. if `--verbose` flag is set. | `True` |

#### <kbd>contextmanager</kbd> `Printer.loading`

```python
msg = Printer()
with msg.loading("Loading..."):
    # Do something here that takes longer
    time.sleep(10)
msg.good("Successfully loaded something!")
```

| Argument | Type | Description | Default |
| --- | --- | --- | -- |
| `text` | unicode | The text to display while loading. | `""` |

#### <kbd>method</kbd> `Printer.table`, `Printer.row`

See [Tables](#tables).

#### <kbd>property</kbd> `Printer.counts`

Get the counts of how often the special printers were fired, e.g.
`MESSAGES.GOOD`. Can be used to print an overview like "X warnings"

```python
msg = Printer()
msg.good("Success")
msg.fail("Error")
msg.warn("Error")

print(msg.counts)
# Counter({'good': 1, 'fail': 2, 'warn': 0, 'info': 0})
```

| Argument | Type | Description |
| --- | --- | --- |
| **RETURNS** | `Counter` | The counts for the individual special message types. |

### Tables

#### <kbd>function</kbd> `table`

Lightweight helper to format tabular data.

```python
from wasabi import table

data = [("a1", "a2", "a3"), ("b1", "b2", "b3")]
header = ("Column 1", "Column 2", "Column 3")
widths = (8, 9, 10)
aligns = ("r", "c", "l")
formatted = table(data, header=header, divider=True, widths=widths, aligns=aligns)
```

```
Column 1   Column 2    Column 3
--------   ---------   ----------
      a1      a2       a3
      b1      b2       b3
```

| Argument | Type | Description | Default |
| --- | --- | --- | --- |
| `data` | iterable / dict | The data to render. Either a list of lists (one per row) or a dict for two-column tables. |  |
| `header` | iterable | Optional header columns. | `None` |
| `footer` | iterable | Optional footer columns. | `None` |
| `divider` | bool | Show a divider line between header/footer and body. | `False` |
| `widths` | iterable / `"auto"` | Column widths in order. If `"auto"`, widths will be calculated automatically based on the largest value. | `"auto"` |
| `max_col` | int | Maximum column width. | `30` |
| `spacing` | int | Number of spaces between columns. | `3` |
| `aligns` | iterable / unicode | Columns alignments in order. `"l"` (left, default), `"r"` (right) or `"c"` (center). If  If a string, value is used for all columns. | `None` |
| **RETURNS** | unicode | The formatted table. |  |

#### <kbd>function</kbd> `row`

```python
from wasabi import row

data = ("a1", "a2", "a3")
formatted = row(data)
```

```
a1   a2   a3
```

| Argument | Type | Description | Default |
| --- | --- | --- | --- |
| `data` | iterable | The individual columns to format. |  |
| `widths` | iterable / `"auto"` | Column widths in order. If `"auto"`, widths will be calculated automatically based on the largest value. | `"auto"` |
| `spacing` | int | Number of spaces between columns. | `3` |
| `aligns` | iterable | Columns alignments in order. `"l"` (left), `"r"` (right) or `"c"` (center). | `None` |
| **RETURNS** | unicode | The formatted row. |  |

### <kbd>class</kbd> `TracebackPrinter`

Helper to output custom formatted tracebacks and error messages. Currently
used in [Thinc](https://github.com/explosion/thinc).

#### <kbd>method</kbd> `TracebackPrinter.__init__`

Initialize a traceback printer.

```python
from wasabi import TracebackPrinter

tb = TracebackPrinter(tb_base="thinc", tb_exclude=("check.py",))
```

| Argument | Type | Description | Default |
| --- | --- | --- | --- |
| `color_error` | unicode / int | Color name or code for errors (passed to `color` helper). | `"red"` |
| `color_tb` | unicode / int | Color name or code for traceback headline (passed to `color` helper). | `"blue"` |
| `color_highlight` | unicode / int | Color name or code for highlighted text (passed to `color` helper). | `"yellow"` |
| `indent` | int | Number of spaces to use for indentation. | `2` |
| `tb_base` | unicode | Name of directory to use to show relative paths. For example, `"thinc"` will look for the last occurence of `"/thinc/"` in a path and only show path to the right of it. | `None` |
| `tb_exclude` | tuple | List of filenames to exclude from traceback. | `tuple()` |
| **RETURNS** | `TracebackPrinter` | The traceback printer. |  |

#### <kbd>method</kbd> `TracebackPrinter.__call__`

Output custom formatted tracebacks and errors.

```python
from wasabi import TracebackPrinter
import traceback

tb = TracebackPrinter(tb_base="thinc", tb_exclude=("check.py",))

error = tb("Some error", "Error description", highlight="kwargs", tb=traceback.extract_stack())
raise ValueError(error)
```

```
  Some error
  Some error description

  Traceback:
  ├─ <lambda> [61] in .env/lib/python3.6/site-packages/pluggy/manager.py
  ├─── _multicall [187] in .env/lib/python3.6/site-packages/pluggy/callers.py
  └───── pytest_fixture_setup [969] in .env/lib/python3.6/site-packages/_pytest/fixtures.py
         >>> result = call_fixture_func(fixturefunc, request, kwargs)
```

| Argument | Type | Description | Default |
| --- | --- | --- | --- |
| `title` | unicode | The message title. |  |
| `*texts` | unicode | Optional texts to print (one per line). |  |
| `highlight` | unicode | Optional sequence to highlight in the traceback, e.g. the bad value that caused the error. | `False` |
| `tb` | iterable | The traceback, e.g. generated by `traceback.extract_stack()`. | `None` |
| **RETURNS** | unicode | The formatted traceback. Can be printed or raised by custom exception. |  |

### Utilities

#### <kbd>function</kbd> `color`

```python
from wasabi import color

formatted = color("This is a text", fg="white", bg="green", bold=True)
```

| Argument | Type | Description | Default |
| --- | --- | --- | -- |
| `text` | unicode | The text to be formatted. | - |
| `fg` | unicode / int | Foreground color. String name or `0` - `256`. | `None` |
| `bg` | unicode / int | Background color. String name or `0` - `256`. | `None` |
| `bold` | bool | Format the text in bold. | `False` |
| **RETURNS** | unicode | The formatted string. | |

#### <kbd>function</kbd> `wrap`

```python
from wasabi import wrap

wrapped = wrap("Hello world, this is a text.", indent=2)
```

| Argument | Type | Description | Default |
| --- | --- | --- | -- |
| `text` | unicode | The text to wrap. | - |
| `wrap_max` | int | Maximum line width, including indentation. | `80` |
| `indent` | int | Number of spaces used for indentation. | `4` |
| **RETURNS** | unicode | The wrapped text with line breaks.

### Environment variables

Wasabi also respects the following environment variables:

| Name | Description |
| --- | --- |
| `ANSI_COLORS_DISABLED` | Disable colors. |
| `LOG_FRIENDLY` | Make output nicer for logs (no colors, no animations). |

## 🔔 Run tests

Fork or clone the repo, make sure you have `pytest` installed and then run it
on the directory [`/tests`](/tests):

```bash
pip install pytest
cd wasabi
python -m pytest tests
```
