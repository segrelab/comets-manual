
# Loading models and CobraPy compatibility

Let's show with a couple of simple simulation how the COMETS Python toolbox deals with models, including compatibility with CobraPy.

We first create a set of parameters by instantiating the params class (with no arguments, meaning that all parameter values will be the default). and a layout containing a simple environment with glucose as the only carbon source. 


```python
import comets as c
import os
import pandas as pd
import matplotlib.pyplot as plt
pd.options.display.max_rows = 10

# .. Create set of parameters with default values, and change timeStep
my_params = c.params()
my_params.all_params['timeStep'] = 0.25

# .. Load layout from file and load cobra model into the layout
my_layout = c.layout('test_models/layout_blueprint')
```
We then load in the Cobra SBML iJO1366 model file to the layout. Each time the models in a layout are changed, we have to run also the `update_models()` method.

```python
my_layout.models = ['test_models/iJO1366.xml']
my_layout.update_models()
my_layout.models
```

    ['iJO1366']

We are now ready toinstantiate the `comets` class and run the simulation.

```python
my_comets = c.comets(my_layout, my_params)
my_comets.run()
```

    Running COMETS simulation ...
    Done!
	
```python
biomass1 = my_comets.total_biomass
plt.plot('cycle', 'iJO1366', data=biomass1)
```
![png](/img/model_testing_4_1.png)


## Adjusting COMETS parameters in models

What if we want to change some parameters of the model? For instance, we might want to change the type of optimization of the model to maximize biomass and then minimize the total flux (parsimonious FBA). 

For manipulating models, we need to load them in SBML format using the `COBRA` package, and then add the corresponding fields for `COMETS` (see bottom of this page). 


```python
import cobra as cb
md = cb.io.read_sbml_model('test_models/iJO1366.xml')
```


```python
# add some parameters to the model
md.obj_style = 'MAX_OBJECTIVE_MIN_TOTAL'

# change the ID of the model
md.id = 'modified_iJO1366'
```

We might also want to change $V_{max}$, $K_m$, and Hill coefficients of some uptake reaction, say glucose uptake. We can add these as fields of the corresponding exchange reaction. However, if we decide to specify these parameters for any particular reaction, we will also need to specify their default values for all the rest of uptake reactions in the model. 

```python
# Set specific values for the glucose transporter
md.reactions.EX_glc__D_e.Vmax = 6
md.reactions.EX_glc__D_e.Km = .1
md.reactions.EX_glc__D_e.Hill = 2

# Set also teh default values for the rest of reactions
md.default_vmax = 10
md.default_km = 0.01
md.default_hill = 1
```

Lets now include this cobra model object in our layout. In addition to passing strings specifying filenames as we did previously, Models can be passed directly as a list of `cobra.Model` objects to the layout:

```python
my_layout.models = [md]
my_layout.models
```

    [<Model modified_iJO1366 at 0x7f3a2b1c59b0>]

NOTE: the list of models can be either a list of model files (either cobra xml or comets models, see below) or a list of cobra models loaded into the environment, but never a mix of both.

In order to make effective any changes in the layout models, we have to run `update_models`:

```python
my_layout.update_models()
```
Now, let's run the new model and compare it to the previous simulation.


```python
my_comets = c.comets(my_layout, my_params)
my_comets.run()
biomass2 = my_comets.total_biomass
plt.plot('cycle', 'iJO1366', data=biomass1)
plt.plot('cycle', 'modified_iJO1366', data=biomass2)
```

    Running COMETS simulation ...
    Done!

![png](/img/model_testing_15_2.png)


Makes sense - we made the model worse in glucose by reducing $V_{max}$ and increasing $K_M$.

## Loading COMETS model files
The `layout` class is also capable of loading model files in COMETS model format (`.cmd`, the format passed to the COMETS Java engine). This will be deprecated in the future to simplify things for the user, who will only have to deal with cobra type SBML models.

For example, let's include a `COMETS` model in our previous layout, and run it. 


```python
my_layout.models = ['test_models/iJO1366.cmd']
my_layout.update_models()
my_layout.models
```

    ['iJO1366']

```python
my_comets = c.comets(my_layout, my_params)
my_comets.run()
```

    Running COMETS simulation ...
    Done!

```python
print(my_comets.run_output)
```

    -script
    running script file: /home/djordje/Dropbox/projects/COMETS-Python-Toolbox/.current_script
    Loading layout file '/home/djordje/Dropbox/projects/COMETS-Python-Toolbox/.current_layout'...
    Found 1 model files!
    Loading 'iJO1366.cmd' ...
	... [etc]

```python
plt.plot('cycle', 'iJO1366', data=my_comets.total_biomass)
```
![png](/img/model_testing_20_1.png)

