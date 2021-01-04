# cometspy.layout module

<!-- !! processed by numpydoc !! -->

### class cometspy.layout.layout(input_obj: list = None)
Bases: `object`

object containing model and environmental definitions for a COMETS sim.

The layout object is an essential object needed to start a comets
simulation, along with a params object. For a simulation to run, the layout
must contain model(s). Typically, the layout will be created after
creating models, and these models will be given in a list during layout
creation. Subsequently, methods like 
set_specific_metabolite() will be used to create environmental media 
definitions.  Additionally, spatial information can be assigned, such as
the spatial dimensions in lattice units (layout.grid), spatial barriers,
etc.


* **Parameters**

    **input_obj**

        either a list of cometspy models, or a path to a previously-written
        COMETS layout to load.


### Examples

```python
>>> import cometspy as c
>>> import cobra.test
>>> e_cobra = cobra.test.create_test_model('textbook')
>>> e = c.model(e_cobra)
>>> e.open_exchanges() 
>>> l = c.layout([e])
>>> # use the media from the textbook in the COMETS media
>>> for key in e_cobra.medium.keys():
>>>     l.set_specific_metabolite(key[3:], 10.)
>>> l.display_current_media()
```


* **Attributes**

    **models**

        a list of cometspy models. do not modify directly. use add_model()

    **grid**

        the number of boxes in the x and y direction. default = [1, 1]

    **media**

        info on met initial values and diffusion. see set_specific_metabolite

    **refresh**

        info on met added per time. see set_specific_refresh

    **local_media**

        info on spatial-explicit media. see set_specific_metabolite_at_location

    **local_refresh**

        info on met added per time to loc. see set_specific_refresh_at_location

    **local_static**

        info on constant met value at loc. see set_specific_static_at_location

    **default_diff_c**

        the baseline diffusion constant for all metabolites

    **barriers**

        list of impenetrable locations. see add_barriers

    **region_map**

        integer array specifying regions. see set_region_map

    **region_parameters**

        region-specific default diffusion params. see set_region_parameters

    **reactions**

        list of extracellular reactions. see add_external_reaction

    **periodic_media**

        list of media undergoing periodic cycles. see set_global_periodic_media


### Methods

| `add_barriers`(barriers)

 | adds impenetrable barriers to the layout at specified locations

 |
| `add_external_reaction`(rxnName, metabolites, …)

 | adds an extracellular reaction to the layout

                    |
| `add_model`(model)

                               | add a cometspy model to this layout

                             |
| `add_typical_trace_metabolites`([amount, static])

 | adds common BIGG-style metabolites to the environment.

          |
| `delete_model_files`(working_dir)

                 | deletes model files in specified directory

                      |
| `display_current_media`()

                         | a utility function to show current non-zero media amounts

       |
| `get_model_ids`()

                                 | returns a list of the ids of the models in this layout.

         |
| `read_comets_layout`(input_obj)

                   | load a comets layout from a file

                                |
| `set_global_periodic_media`(metabolite, …)

        | set cyclical changes in extracellular nutrient environment

      |
| `set_metabolite_diffusion`(diffusion_constant)

    | sets the default diffusion constant for all metabolites

         |
| `set_region_map`(region_map)

                      | set a region_map dictating different regions in a spatial simulation

 |
| `set_region_parameters`(region, diffusion, …)

     | set regions-specific diffusion and friction values

                   |
| `set_specific_metabolite`(met, amount[, static])

  | sets the initial (or constant) value for a metabolite across space

   |
| `set_specific_metabolite_at_location`(met, …)

     | set an initial value of a metabolite at a specific spatial location

  |
| `set_specific_metabolite_diffusion`(met, …)

       | sets the diffusion constant of a specific metabolite

                 |
| `set_specific_refresh`(met, amount)

               | set the amount to increment a metabolite continuously through time

   |
| `set_specific_refresh_at_location`(met, …)

        | sets the amount to adjust a metabolite at a specific location in time

 |
| `set_specific_static`(met, amount)

                | sets a metabolite to a fixed value in every location

                  |
| `set_specific_static_at_location`(met, …)

         | sets a metabolite to a fixed value at a specific location

             |
| `update_models`()

                                 | updates layout properties when models are added / removed

             |
| `write_layout`(working_dir)

                       | writes just the COMETS layout file to the supplied path

               |
| `write_model_files`([working_dir])

                | writes each model file

                                                |
| `write_necessary_files`(working_dir)

              | writes the layout and the model files to file

                         |
<!-- !! processed by numpydoc !! -->

#### add_barriers(barriers: list)
adds impenetrable barriers to the layout at specified locations

Barriers can be used in COMETS to create impenetrable spatial
locations, for example to simulate rocks or to make a rounded
simulation. Neither biomass nor metabolites can diffuse through
barriers.

In COMETS, locations are zero-ordered.


* **Parameters**

    **barriers**

        A list containing tuples of integers of x,y locations.


### Examples

```python
>>> # make a diagonal line of barriers
>>> l = c.layout([model1, model2]) # assumes model1,2 are made
>>> l.grid = [5,5]
>>> l.add_barriers([(0,0),(1,1),(2,2),(3,3),(4,4)])
```

<!-- !! processed by numpydoc !! -->

#### add_external_reaction(rxnName: str, metabolites: list, stoichiometry: list, \*\*kwargs)
adds an extracellular reaction to the layout

External reactions are reactions not tied to a growing (living) model.
They have rates defined in certain kwargs. There is no enyme
concentration; it is assumed fixed.


* **Parameters**

    **rxnName**

        name of the external reaction

    **metabolites**

        list of metabolite names (strs)

    **stoichiometry**

        stoichiometry of the metabolites. must be same order as metabolites

    **\*\*kwargs**

        rate parameters. Either Kcat and Km, or K must be set.


### Examples

```python
>>> # set an external reaction that converts lactose into glucose + gal
>>> # assumes import cometspy as c and a model called m has been made
>>> rxn_name = 'lactase'
>>> metabolites = ['lcts_e', 'glc__D_e', 'gal_e']
>>> stoichiometry = [-1., 1., 1.]
>>> K = 0.2 # mmol / hr
>>> l = c.layout([m]) # m is a cometspy.model that has exchanges for the mets above
>>> l.add_external_reaction(rxn_name, metabolites, stoichiometry,
                            K = K)
```

<!-- !! processed by numpydoc !! -->

#### add_model(model)
add a cometspy model to this layout

This is the preferred way to add models to existing layout.


* **Parameters**

    **model**

        a cometspy.model with an initial_pop


### Examples

```python
>>> import cobra.test
>>> import cometspy as c
>>> layout = c.layout()
>>> ecoli = cobra.test.create_test_model("ecoli")
>>> salmonella = cobra.test.create_test_model("salmonella")
>>> ecoli = c.model(ecoli)
>>> salmonella = c.model(salmonella)
>>> ecoli.open_exchanges()
>>> salmonella.open_exchanges()
>>> ecoli.initial_pop = [0, 0, 1.e-7]
>>> salmonella.initial_pop = [0, 0, 1.e-7]
>>> layout.add_model(ecoli)
>>> layout.add_model(salmonella)
```

<!-- !! processed by numpydoc !! -->

#### add_typical_trace_metabolites(amount: float = 1000.0, static: bool = True)
adds common BIGG-style metabolites to the environment. This only 
works if your models use metabolites which are formatted like ‘ca2_e’
and then should still only be used as a starting point.


* **Parameters**

    **amount**

        the amount, in mmol, of these metabolites. Default is 1000.0.

    **static**

        The default is True. If false, these metabolites are depletable.


<!-- !! processed by numpydoc !! -->

#### delete_model_files(working_dir)
deletes model files in specified directory

<!-- !! processed by numpydoc !! -->

#### display_current_media()
a utility function to show current non-zero media amounts

<!-- !! processed by numpydoc !! -->

#### get_model_ids()
returns a list of the ids of the models in this layout.

<!-- !! processed by numpydoc !! -->

#### read_comets_layout(input_obj: str)
load a comets layout from a file

If a COMETS layout file has been previously made, this function can
load that file into the current cometspy layout object.

This is an unusual way of working with cometspy with rare uses.


* **Parameters**

    **input_obj**

        the path to the existing COMETS layout file to be loaded


<!-- !! processed by numpydoc !! -->

#### set_global_periodic_media(metabolite: str, function: str, amplitude: float, period: float, phase: float, offset: float)
set cyclical changes in extracellular nutrient environment


* **Parameters**

    **metabolite**

        name of the metabolite under a cycle

    **function**

        name of function that defines changing metabolite concentration

    **amplitude**

        amplitude of the function in mmol

    **period**

        duration of the function period in hr

    **phase**

        horizontal phase of the period (hr)

    **offset**

        vertical offset of the function (mmol)


<!-- !! processed by numpydoc !! -->

#### set_metabolite_diffusion(diffusion_constant: float)
sets the default diffusion constant for all metabolites

Specific diffusion constants can be set subsequent to setting this.


* **Parameters**

    **diffusion_constant**

        the diffusion constant, in cm2 / s, that all metabolites use


<!-- !! processed by numpydoc !! -->

#### set_region_map(region_map: numpy.array)
set a region_map dictating different regions in a spatial simulation

COMETS can have different regions with different substrate diffusivities
and frictions.  Here, you set the map defining the regions. Specifically,
you provide either:

> 
> 1. a numpy array whose shape == layout.grid, or


> 2. a list of lists whose first length is grid[0] and second len is grid[1]

Populating these objects should be integer values, beginning at 1 and
incrementing only, that define the different grid areas.  These are
intimately connected to region_parameters, which are set with
layout.set_region_parameters()


* **Parameters**

    **region_map**

        a 2d array of size layout.grid containing integers, or a list of
        lists which can be coerced to an integer array using np.array()


### Examples

see set_region_parameters for an example

<!-- !! processed by numpydoc !! -->

#### set_region_parameters(region: int, diffusion: list, friction: float)
set regions-specific diffusion and friction values

COMETS can have different regions with different substrate 
diffusivities and frictions.  Here, you set those parameters.

This does not affect a simulation unless a region map is also set, 
using the layout.set_region_map() function.


* **Parameters**

    **region**

        the region (set by set_region_map) that uses the other parameters

    **diffusion**

        a list containing diffusion constants (cm2/s) for each metabolite

    **friction**

        a single value for the friction observed in this region


### Examples

```python
>>> # this example assumes you have already created a cometspy model
>>> # called "e" and have imported cometspy as c and numpy as np
>>> # Here we are making a 2x2 world with two regions in which region
>>> # 1 metabolites have diffusion constant 1.e-6 cm2/s and in region
>>> # 2 metabolites have diffusion constant 1.e-7 cm2/s, and in both
>>> # cases mets have default friction constant 1.
>>> l = c.layout([e])
>>> l.grid = [2,2]
>>> region_map = np.array([[1, 1], [2, 2]], dtype = int)
>>> l.set_region_map(region_map)
>>> n_mets = length(l.media) # how many metabolites there are
>>> l.set_region_parameters(1, [1.e-6] * n_mets, 1.)
>>> l.set_region_parameters(1, [1.e-7] * n_mets, 1.)
```

<!-- !! processed by numpydoc !! -->

#### set_specific_metabolite(met: str, amount: float, static: bool = False)
sets the initial (or constant) value for a metabolite across space

This is the most common way to set a metabolite concentration. It 
sets the initial value of the named metabolite to ‘amount’ in every
location. Optionally, if static = True, this concentration is fixed
during the simulation.


* **Parameters**

    **met**

        name of the metabolite

    **amount**

        initial value for the metabolite in each box, in mmol

    **static**

        DEFAULT = False. If True, the ‘amount’ is fixed over time.


### Examples

```python
>>> l = c.layout([model])
>>> l.set_specific_metabolite("glc__D_e", 0.005)
>>> l.set_specific_metabolite("o2_e", 15, static = True)
>>> # a common thing to do is populate this from the cobra model, as 
>>> # shown here:
>>> import cobra.test
>>> import cometspy as c
>>> cobra_model = cobra.test.create_test_model("textbook")
>>> model = c.model(cobra_model)
>>> model.open_exchanges()
>>> l = c.layout([model])
>>> for key, value in cobra_model.medium.items():
>>>     l.set_specific_metabolite(key[3:], value)
```

<!-- !! processed by numpydoc !! -->

#### set_specific_metabolite_at_location(met: str, location: tuple, amount: float)
set an initial value of a metabolite at a specific spatial location

In contrast to set_specific_metabolite, which sets initial values 
universally over space, this method sets a specific metabolite 
initial value at one location only.


* **Parameters**

    **met**

        name of the metabolite

    **location**

        a tuple of integers specifying a specific location, e.g. (3,2)

    **amount**

        the initial value for the metabolite at the location, in mmol


### Notes

Remember that COMETS locations are zero-ordered.

### Examples

```python
>>> l = c.layout([model])
>>> l.grid = [10,1]
>>> l.set_specific_metabolite_at_location("glc__D_e", (9,0), 0.005)
```

<!-- !! processed by numpydoc !! -->

#### set_specific_metabolite_diffusion(met: str, diffusion_constant: float)
sets the diffusion constant of a specific metabolite


* **Parameters**

    **met**

        name of the metabolite

    **diffusion_constant**

        the diffusion constant, in cm2/s, of the named metabolite


### Examples

```python
>>> l = c.layout([m])
>>> l.set_specific_metabolite_diffusion("ac_e", 5.2e-6)
>>> l.set_specific_metabolite_diffusion("gal_e", 4.e-6)
```

<!-- !! processed by numpydoc !! -->

#### set_specific_refresh(met: str, amount: float)
set the amount to increment a metabolite continuously through time

metabolites can be “refreshed” or “dripped in” during a COMETS 
simulation. This sets the amount added each hour, which is divided
up by the number of time steps. It applies that refresh to all boxes
in space.


* **Parameters**

    **met**

        name of the metabolite refreshed

    **amount**

        the amount added to (or subtracted from) each box, in mmol / hr.


<!-- !! processed by numpydoc !! -->

#### set_specific_refresh_at_location(met: str, location: tuple, amount: float)
sets the amount to adjust a metabolite at a specific location in time

Like set_specific_refresh, but for a specific lacation. This 
Method is used to specify how much a metabolite is increased (or 
decreased) each hour in a specific location.


* **Parameters**

    **met**

        the name of the metabolite

    **location**

        the (x, y) location to adjust, with x and y of type int

    **amount**

        the amount added to (or subtracted from) the box, in mmol / hr


<!-- !! processed by numpydoc !! -->

#### set_specific_static(met: str, amount: float)
sets a metabolite to a fixed value in every location


* **Parameters**

    **met**

        name of the metabolite

    **amount**

        the amount, in mmol, that is in each box at each time step


<!-- !! processed by numpydoc !! -->

#### set_specific_static_at_location(met: str, location: tuple, amount: float)
sets a metabolite to a fixed value at a specific location


* **Parameters**

    **met**

        name of the metabolite

    **location**

        the (x, y) location of the fixed metabolite amount. x, y are int

    **amount**

        the amount, in mmol, that is in each box at each time step


<!-- !! processed by numpydoc !! -->

#### update_models()
updates layout properties when models are added / removed

This is usually just called internally. However, if you manually 
change layout.models, then this method should be called to make other
necessary changes.

<!-- !! processed by numpydoc !! -->

#### write_layout(working_dir: str)
writes just the COMETS layout file to the supplied path


* **Parameters**

    **working_dir**

        the path to the directory where .current_layout will be written


<!-- !! processed by numpydoc !! -->

#### write_model_files(working_dir='')
writes each model file

<!-- !! processed by numpydoc !! -->

#### write_necessary_files(working_dir: str)
writes the layout and the model files to file

This method is called by comets.run(). Alternatively it may be called
directly if one wishes to inspect comets layout and model files.

A path must be specified if called directly.

Note: the layout file will be called “.current_layout” and therefore
be hidden from file browsers by default.


* **Parameters**

    **working_dir**

        The directory where the files will be written.


<!-- !! processed by numpydoc !! -->
