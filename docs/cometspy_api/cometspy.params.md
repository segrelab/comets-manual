# cometspy.params module

<!-- !! processed by numpydoc !! -->

### class cometspy.params.params(global_params: str = None, package_params: str = None)
Bases: `object`

object storing simulation-related and some default biological parameters

The params object is an essential object needed to start a comets
simulation, along with a layout object containing models. The params 
object stores simulation information such as timeStep and maxCycles, as 
well as many default biological parameters such as defaultKm and 
defaultDiffC (the default metabolite diffusion constant).

All of the parameters are stored in a dictionary called ‘all_params’ and
can either be adjusted directly or using set_param().

When params are written to a COMETS object for COMETS to process, two
files are generated (‘.current_global’ and ‘current_package’)


* **Parameters**

    **global_params**

        optional path to an existing ‘global_params’ file to load

    **package_params**

        optional path to an existing ‘package_params’ file to load


### Examples

```python
>>> # make a params object and change two params
>>> import cometspy as c
>>> params = c.params()
>>> params.set_param("timeStep", 0.01) # hours
>>> params.set_param("spaceWidth", 0.5) # cm
>>> # view all the params
>>> params.show_params()
>>> # access the params directly
>>> params.all_params # this is a dict
>>> # use a params object in a comets simulation
>>> import cobra.test
>>> model = c.model(cobra.test.create_test_model("textbook"))
>>> model.initial_pop = [0, 0, 1.e-7]
>>> model.open_exchanges()
>>> layout = c.layout([model])
>>> # normally we'd now alter the media in the layout
>>> layout.add_typical_trace_metabolites()
>>> layout.set_specific_metabolite('lcts_e', 0.05) # mmol
>>> sim = c.comets(layout, params)
>>> sim.run()
```


* **Attributes**

    **all_params**

        contains every possible param as keys, with their value as the value

    **params_units**

        dictionary containing the units for each possible parameter


### Methods

| `get_param`(name)

 | returns the value associated with the given parameter name

 |
| `set_param`(name, value)

                            | alters a specific parameter value

                                     |
| `show_params`()

                                     | utility function to show all possible parameters and their units

      |
| `write_params`(out_glb, out_pkg)

                    | writes params data to the specified files

                             |
<!-- !! processed by numpydoc !! -->

#### get_param(name: str)
returns the value associated with the given parameter name


* **Parameters**

    **name**

        the name of the parameter whose value is desired



* **Returns**

    object

        the type of object depends on the parameter requested


<!-- !! processed by numpydoc !! -->

#### set_param(name: str, value)
alters a specific parameter value

See show_params() for a list of possible parameters to adjust.


* **Parameters**

    **name**

        the name of a specific parameter to change

    **value**

        the type of the value depends on the parameter.


### Examples

```python
>>> p = cometspy.params()
>>> p.set_param("writeBiomassLog", True)
>>> p.set_param("maxCycles", 100)
>>> p.set_param("BiomassLogRate",5)
>>> p.set_param("defaultVmax", 10.24)
```

<!-- !! processed by numpydoc !! -->

#### show_params()
utility function to show all possible parameters and their units

<!-- !! processed by numpydoc !! -->

#### write_params(out_glb: str, out_pkg: str)
writes params data to the specified files

Usually this is only used internally, though a user could use it to 
pre-generate params files.


* **Parameters**

    **out_glb**

        the path and name of the ‘global’ parameters file to be written

    **out_pkg**

        the path and name of the ‘package’ parameters file to be written


<!-- !! processed by numpydoc !! -->
