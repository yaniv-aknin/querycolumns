# querycolumn

![Build Status](https://github.com/yaniv-aknin/querycolumns/actions/workflows/test.yml/badge.svg?branch=main)

Simple extension to [Pandas](https://pandas.pydata.org/) that makes it easier to select columns in (very) [wide](https://en.wikipedia.org/wiki/Wide_and_narrow_data) DataFrames. If you name your columns in a hierarchical fashion with a separator (e.g, as you might get from `pd.normalize_json()`, it lets you select a column or group of columns easily and with tab completion.

Here's a quick demo of what this looks like.

```
    >>> import pandas as pd
    >>> from querycolumns import patch_dataframe_with_query_columns
    >>> patch_dataframe_with_query_columns()
    >>> df = pd.DataFrame([{'person.size.height': 3, 'person.size.weight': 4}])
    >>> df.qc.person
    <QueryColumns @ person: size>
    >>> df.qc.person.size
    <QueryColumns @ person.size: height, weight>
    >>> df.qc.person.size.weight
    0    4
    Name: person.size.weight, dtype: int64
    >>> df[df.qc.person.size]
       person.size.height  person.size.weight
    0                   3                   4
    >>> 
```

QueryColumns does its magic by patching a [Descriptor](https://docs.python.org/3/howto/descriptor.html) into the `DataFrame` class. When you retrieve the attribute `qc` from a frame, `QueryColumns.__get__()` is invoked; this is when QueryColumns introspects its parent dataframe and returns a magical object with tab completion for segmented column names.

Note I wrote QueryColumns after working with Pandas for about a week. I don't know if it encourages elegant use of the library, but I find it useful for my usecase. I'm practically certain that it doesn't extend Pandas as one should (there are [explicit](https://pandas.pydata.org/pandas-docs/stable/development/extending.html) APIs for that), but I couldn't think of a way to do it without Descriptors.
