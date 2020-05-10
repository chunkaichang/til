# Plotting bar graphs with pandas

The following code plots a bar graph with 5 groups, each of which has 2 attributes.

Note that the plot function returns an `matplotlib.axes.Axes` object, which can be used to set xlabel and ylabel.

## Additional features

- Stacked bars: specifying `stacked=True` in `plot.bar()` will stack attributes of each group.
- Subplots by attributes: specifying `subplots=True` will split the original graph into subplots by attributes. Notice that `plot.bar()` will return an array of axes objects instead.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

groups = list('vwxyz')
attr1 = list(np.arange(1,6))
attr2 = list(np.arange(5,0,-1))

df2 = pd.DataFrame({'attr1': attr1, 'attr2': attr2}, index=groups)

ax = df2.plot.bar(title='mygraph')
ax.set(xlabel='groups', ylabel='values')
```

---
## References
[Pandas visualization docs](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization-barplot)
[Bar plot API](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.bar.html)
