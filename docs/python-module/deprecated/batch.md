
# Batch culture in COMETS

One of the most common experimental designs in microbial ecology and evolution consists on periodically transfering a small amount of a grown bacterial culture to fresh media, which is known as "batch culture".

In `COMETS`, this is easily done using three parameters
 * `batchDilution` is either True or False; specifies whether periodic dilutions are performed. 
 * `dilTime` is the periodicity at which dilutions are performed, in hr. 
 * `dilFactor` is the dilution factor applied. If the value passed here is larger than one, that will be the dilution factor; if it is smaller than one, it will be the applied dilution properly. For instance, a 1:100 dilution can be specified as either 100 or 0.01. 

Let's first do a batch culture with no oxygen in the environment. 


```python
import comets as c
import os
import pandas as pd
import cobra as cb
pd.options.display.max_rows = 10

# Set relevant parameters
b_params = c.params()
b_params.all_params['batchDilution'] = True
b_params.all_params['dilTime'] = 24
b_params.all_params['dilFactor'] = 100
b_params.all_params['timeStep'] = .2
b_params.all_params['maxCycles'] = 400

# Set layout
b_layout = c.layout('test_batch/batch_layout')
b_layout.media.loc[b_layout.media['metabolite'] == 'glc__D_e', 'init_amount'] = 20
b_layout.media.loc[b_layout.media['metabolite'] == 'o2_e', 'init_amount'] = 0
b_layout.media
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>metabolite</th>
      <th>init_amount</th>
      <th>diff_c</th>
      <th>g_static</th>
      <th>g_static_val</th>
      <th>g_refresh</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>o2_e</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>glc__D_e</td>
      <td>20.0</td>
      <td>NaN</td>
      <td>0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ca2_e</td>
      <td>1000.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>cbl1_e</td>
      <td>1000.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>cl_e</td>
      <td>1000.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>sel_e</td>
      <td>1000.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>slnt_e</td>
      <td>1000.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>so4_e</td>
      <td>1000.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>tungs_e</td>
      <td>1000.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1000.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>zn2_e</td>
      <td>1000.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>1000.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>25 rows × 6 columns</p>
</div>


Now, run simulation and draw the biomass curve. 


```python
# create comets object from the loaded parameters and layout 
batch_anaerob = c.comets(b_layout, b_params)

# run comets simulation
batch_anaerob.run()
```

    Running COMETS simulation ...
    Done!


```python
batch_anaerob.total_biomass.plot(x = 'cycle', y = 'iJO1366')
```

![png](/img/batch_4_1.png)

Here, we see that the population is unable to reach enough density in a single cycle to maintain a stable population, an is therefore going to extinction. 

Lets now add oxygen to the environment, and redo the simulation. 


```python
b_layout.media.loc[b_layout.media['metabolite'] == 'o2_e', 'init_amount'] = 1000
```


```python
# create comets object from the loaded parameters and layout 
batch_aerob = c.comets(b_layout, b_params)

# run comets simulation
batch_aerob.run()
```

    Running COMETS simulation ...
    Done!


```python
batch_aerob.total_biomass.plot(x = 'cycle', y = 'iJO1366')
```

![png](/img/batch_8_1.png)


Now, with oxygen in the environment enabling respiration, the growth rate is much higher, and the population soon reaches stationnary phase, maintaining the population through the successive dilutions. 
