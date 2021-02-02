---
title: Maximize the cell size of Jupyter notebook
draft: false
date: "2020-11-29"

categories:
- Dev
tags:
- Python
- Jupyter
---
### TL;DR

Put the following magic command (cell magic) in a cell and execute it.

```python
%%javascript
IPython.OutputArea.auto_scroll_threshold = 9999;
```


Note: This is not `line magic` like `% matplotlib inline` and some errors will occur unless the executed cell is independent.
See [IPython Official Documentation](https://ipython.readthedocs.io/en/stable/interactive/magics.html) for details.

- `%` - All available *line magic* commands - [check here](https://ipython.readthedocs.io/en/stable/interactive/magics.html#line-magics)
- `%%` - All available *cell magic* commands - [check here](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cell-magics)


### Cf.

- [Magic functions - IPython公式ドキュメント](https://ipython.readthedocs.io/en/stable/interactive/tutorial.html#magic-functions)
- [comment:53708976 issue:2172 ipython github.com](https://github.com/ipython/ipython/issues/2172#issuecomment-53708976)
- [ipythonノートブックの出力ウィンドウのサイズを変更する](https://qastack.jp/programming/18770504/resize-ipython-notebook-output-window)
- [Jupyter notebookで列をすべて表示したい - Qitta](https://qiita.com/daifuku_mochi2/items/30258e58750ff8e85d37)


