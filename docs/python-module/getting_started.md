# The Comets Python module

The Comets python module is intended to offer a programatic, yet easy and intuitive way to use Comets. While it internally uses the Comets Java engine, it replaces parameter files, simulation layouts or models, bash scripts or output files by python objects that users deal with to both perform simulations and analyze the results. 

Any comets simulation starts from a **layout** and a set of **parameters**. The layout specifies the environment (media metabolites, refresh values, periodic dilutions) and the species present in it, that is, the **models**. The parameters specify many simulation characteristics, such as number of iterations, timestep, type of metabolite exchange or whether to record different output logs. Therefore, the three main types of objects are `layout`, `parameters` and `model`, all of them integrated in `comets` type objects which perform simulations and contain their output. 

# The `model` class 
The model class adapts a genome-scale model to be used with comets. It is instantiated with one argument, specifying either a path for a comets model file, a cobra model file, or a cobra model object. Note that for cobra compatibilty, obviously the cobra python package must be installed (see instructions here). For now, let's use the E. coli comets model file bundled with Comets. 

```python
import comets as c

ecoli = c.model('ecoli.cmd')
```
Models contain three basic fields: The reactions and their bounds, the metabolites, and the S matrix (in sparse form). 

In addition, Comets models have a field containingthe model in Cobra format, making it easy to switch between Comets and Cobra functionalities. For instance, one might want to do a quick Flux Balance Analysis of the `ecoli` model: 

```python
ecoli.cobra_model.run()
```
Models can also be written in Comets format using the `write_model` method (for instance, `ecoli.write_model()`) although this method is mostly intended for internal use. 



# The `params` class
The parameters class is a `dict` with the parameter names and values for a simulation. If we initialize the class without arguments, it will contain the default parameter values (see [here]()): 

```python
my_params = c.params()
```

The class can also load comets parameters files (see the definition of these files [here]()). The arguments must be, in that order, the two filenames corresponding to *global_params* and *package_params*:

```python
my_params = c.params('global_params_file_path', 'package_params_file_path')
```

The parameters dict can be modified as desired. 
```python
my_params.all_params['maxCycles'] = 100
```

The `params` class also contains a method to write these parameters in files with the correct format to be fed to the Java Comets engine, but this will be rarely used when working within the Comets Python environment. 

```
my_params.write_params('global_params_outfile', 'package_params_outfile')
```


# The `layout` class
The layout class describes the characteristics of the environment, i.e. the "world". Empty layouts can be created if the class is instantiated without arguments as `my_layout = c.layout()` but the preferred way to instantiate a layout is either providing a `model` (or several of them), or a [Comets layout file](). If fed `models`, it will create layouts with all media components those models are able to exchange with the environment, and default values for everything else. 

To examine the different parts of a Comets `layout`, let's first create one from the standard *E. coli* model bundled with Comets. 

```python
my_layout = c.layout('ecoli_model')
```
Firstly, the layout stores information about the species (`ec_layout.models`) and spatial structure (`ec_layout.grid`) in the environment. In this case, the model is only the *E. coli* one and the grid is the default one, only one cell.

TODO: add models to layout.

The layout stores also information about the **media** as a pandas dataframe:
```python

ec_layout.media
```
| metabolite | init_amount | diff_c | g_static | g_static_val | g_refresh_val |
|------------|-------------|--------|----------|--------------|---------------|
| ca2_e      | 1000        | NaN    | 1        | 1000         | 0             |
| cbl_e      | 1000        | NaN    | 1        | 1000         | 0             |
| cl_e       | 1000        | NaN    | 1        | 1000         | 0             |
| . . .      | . . .       | . . .  | . . .    | . . .        | . . .         |


When initiated from models, the media compounds that can be in the environment are all those for which there is an exchange reaction in at least one of the models. The media table is a pandas dataframe where several pieces of information are stored:

* `init_amount` is the initial amount to be added to each cell of the simulation grid (in mmol). 
* `diff_c` indicates whether the molecule has a diffusion constant different than the default one (stored in `ec_layout.global_diff`)
* `g_static` indicates whether the component should remain at a static value, i.e. without change due to consumption and other effects of the simulation. This is useful for example, for setting some nutrients as unlimited. 
* `g_static_val` indicates at which value shouold the nutrient remain static, if the previous coulmn value is 1. 
* `g_refresh_val` indicates the amount of the metabolite that should be added after each simulation cycle to each cell of the grid. 


In addition, we can set **local "static"** and **"refresh"** values, i.e. specific to a cell of the simulation grid. 

Local refresh values are stored in a list, `ec_layout.local_refresh`, where each element of the list is itself a list with the form `[ x  y  m1_r  m2_r  m3_r ... ]`. The first two elements `x` and `y` represent the coordinates, and are followed by the refresh values for all metabolites, in the same order as in `media`.

Local static values are stored in a similar way. Each element of the `ec_layout.local_static` list is itself a list with the form `[ x  y  m1_s  m1_s_v m2_s  m2_s_v ... ]`. The difference here is that for each metabolite, there are two values, one defining whether the molecule is to be static at that coordinate (`m1_s`, `m2_s`, ... ) and another with the value at which it should be kept (`m1_s_v`, `m2_s_v`, ... ). 

Note that both `local_refresh` and `local_static` can be empty (the default), or contain only entries for the coordinates where there is at least one nonzero refresh or static value, respectively. 

Finally, the layout also contains information about the starting biomass of each model. This information is stored in two fields, `initial_pop_type` and `initial_pop`. 

* **If `initial_pop_type` is `random`**: 
    * `initial_pop` is a list with the structure `[N_i biomass_1 N_j biomass_2 ...]`. This will randomly assign `N_i` spaces to have `biomass_1` amount of model 1, and so on. 

* **If `initial_pop_type` is `random_rect`**: 
    * `initial_pop` is a list with the structure `[x y w h N_i biomass_1 N_j biomass_2 ...]`. As above, but the randomly assigned biomass spots are confined to a rectangle with its upper-left corner at grid space (X,Y), with W grid spaces wide, and H spaces tall. 
* **If `initial_pop_type` is `filled`**: 
    * `initial_pop` is a list with the structure `[biomass_1 biomass_2 ...]`. This will completely fill the grid with the given amount of biomass from each species.
* **If `initial_pop_type` is `filled_rect`**: 
    * `initial_pop` is a list with the structure `[x y w h biomass_1 biomass_2 ...]`. This will completely fill the rectangle defined by `X`, `Y`, `W` and `H` with the given amount of biomass from each species.
* **If `initial_pop_type` is `square`**: 
    * `initial_pop` is a list with the structure `[N_i biomass_1 N_j biomass_2 ...]`. This will fill squares with radius `N_i` at the center of the grid with the amount of biomass specified for each species.
* **If `initial_pop_type` is `custom`**: 
    * initial_pop` is a list of lists, in which each entry is itself a list `[x y biomass_1 biomass_2 ...]`specifying the amount of biomass of each species for each coordinate. 


# The `comets` class
This class represents the core of the module. It integrates the layout and the parameters, performs the simulation, and stores the output of it. Once comets is properly [installed]() and layouts and parameters defined, running a comets simulation is pretty straightforward. We firstly define the `comets` object giving it a `layout` and a `params` objects as arguments. Then, we `run()` the simulation: 

```python
my_simulation = c.comets(my_layout, my_params)
my_simulation.run()
```

In the background, this actually invokes the `COMETS` Java engine in a console, giving an output (stdout) and possibly errors (stderr) both of which can be acessed through the fields `run_outputs` and `run_errors`, respectively. 

```python
print(my_comets.run_output)
```
```
-script
running script file: '/home/djordje/Dropbox/COMETS_RUN/.current_script'
Loading layout file '/home/djordje/Dropbox/COMETS_RUN/.current_layout'...
Found 1 model files!
Loading 'EC_ijo1366_model' ...
...
...
```

```python
print(my_comets.run_errors)
```
```
STDERR is empty.
```

The results of the successful simulation is stored as well in several fields in the `comets` object, depending on whether the parameters `writeTotalBiomasslog`,  `writeBiomassLog`, `writeFluxLog` and `writeMediaLog` were set to `True`. All of the output files ae `pandas` dataframes:

* The field `total_biomass`  stores the total biomass (summed up over all coordinates) for each timepoint and species.
* The field `biomass` stores detailed biomass values for each timepoint, coordinate and species. 
* The field `media` stores the composition of the media at each timepoint.
* The field `fluxes` stores the metabolic fluxes for each species, coordinate and timepoint. 

Additionally, specific comets modes will have additional output fields; for instance, if we run an evolution simulation, the field `genotypes` will store information about each species such as its ancestor and which mutation it suffered. For a more detailed description of th output files, see the different examples below. 
