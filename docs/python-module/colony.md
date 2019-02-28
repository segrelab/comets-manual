# Simulating the growth of a bacterial colony

One of the most simple examples of COMETS capabilities is the simulation of the growth of a bacterial colony. 

```python
import comets as c
import os
import pandas as pd
pd.options.display.max_rows = 10
```

We first use the `params` class to load some pre-set parameters files. This class can also be initialized without arguments, returning a valid `params` object with all parameters set to their default value. 

```python
my_params = c.params('colony/global_params', 'colony/package_params')
my_params.all_params['maxCycles'] = 300
my_params.all_params['writeMediaLog'] = False
```

Then, we load a layout file for our simulation. In this case, it is a 50 x 50 grid with 1e-9 grams of biomass in coordinate [25, 25], that is, in the center of the grid. 

```python
my_layout = c.layout('colony/Ecoli_colony_layout')
my_layout.initial_pop
```
```
[[25.0, 25.0, 1e-07]]
```

Finally, we can create a `comets` object with our `params` and `layout` as inputs, and run it. 
```python
my_comets = c.comets(my_layout, my_params)
my_comets.run()

```
```
Running COMETS simulation ...
Done!
```
Now we can analyze the data. For instance, we can use the `biomass` field to plot a heatmap of the simulation endpoint, our little *E. coli* colony. 

```python
import numpy as np

biomass_final = np.zeros((my_layout.grid[0], my_layout.grid[1]))

end_timepoint = my_comets.biomass.loc[my_comets.biomass['Cycle'] == 400]
for index, row in end_timepoint.iterrows():
    biomass_final[row['x'], row['y']] = row['biomass']
	
import matplotlib.pyplot as plt
import matplotlib.cm, matplotlib.colors
import copy

my_cmap = copy.copy(matplotlib.cm.get_cmap('gray')) # copy the default cmap
my_cmap.set_bad((0,0,0))
plt.imshow(biomass_END, interpolation='nearest', 
           norm=matplotlib.colors.LogNorm(), cmap=my_cmap)
```
![colony](img/colony.png)
