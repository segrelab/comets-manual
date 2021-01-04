# cometspy.utils module

The utils module contains helper functions generating spatial patterns.

<!-- !! processed by numpydoc !! -->

### cometspy.utils.chemostat(models: list, reservoir_media: dict, dilution_rate: float)
helper function to let a user skip some steps when generating a chemostat

This sets relevant simulation parameters (e.g. deathRate, 
metaboliteDilutionRate) and layout values (e.g. refresh media) based upon
a “reservoir” definition and a dilution rate.

It generates a layout that has the reservoir_media as the initial values,
as well as set it to drip in / out based upon the dilution rate.

The returned layout and params can be further modified before supplying
to a comets object if desired.


* **Parameters**

    **models**

        list of cometspy.model(s) with initial_pop set to use in the sim

    **reservoir_media**

        media definition with metabolite names as keys as mmol amounts as values

    **dilution_rate**

        the dilution rate of the chemostat, in 1/hr



* **Returns**

    tuple (layout, params)

        a cometspy.layout object and a cometspy.params object


### Examples

```python
>>> import cobra.test
>>> import cometspy as c
>>> from cometspy.utils import chemostat
>>> # make a model from a cobra model, open exchange reactions, and give a pop
>>> tb = cobra.test.create_test_model("textbook")
>>> m = c.model(tb)
>>> m.initial_pop = [0, 0, 1.e-4]
>>> m.open_exchanges()
>>> reservoir = {'glc__D_e' : 0.01, 'nh4_e' : 1000., 'pi_e' : 1000.}
>>> layout, params = chemostat([m], reservoir, 0.1)
>>> params.set_param("maxCycles", 100)
>>> sim = c.comets(layout, params)
>>> sim.run()
>>> print(sim.total_biomass)
```

<!-- !! processed by numpydoc !! -->

### cometspy.utils.grow_rocks(n: int, xrange: tuple, yrange: tuple, mean_size: int)
grows simple simulated rocks by adding random adjacent points from seeds

n number of seed points are generated first with pick_random_locations. 
Then, mean_size \* n - n additional points are added to these seed locations. 
For each new point, one random location out of the set of all possible 
unoccupied locations next to occupied locations are chosen. Only 
lattice points directly to the NSEW are considered. This process is
repeated until all new points are assigned. This usually results in
‘rocks’ of different shapes and sizes.

This function can be very slow (> 1 min) when n \* mean_size > 2000


* **Parameters**

    **n**

        number of seed points to generate rocks from

    **xrange**

        x range possible, e.g. (0, 5)

    **yrange**

        y range possible, e.g. (0, 10)

    **mean_size**

        average size in lattice units of a generated rock



* **Returns**

    list

        list of all points generated including seed points


<!-- !! processed by numpydoc !! -->

### cometspy.utils.pick_random_locations(n: int, xrange: tuple, yrange: tuple, forbidden_locs: set = {})
returns a list of n x,y tuples corresponding to locations in the range given


* **Parameters**

    **n**

        number of locations desired

    **xrange**

        the x-range (min, max) of the x range possible

    **yrange**

        the y-range (min, max) of the y range possible

    **forbidden_locs**

        A list of tuples that cannot be chosen.



* **Returns**

    list

        a list of (x,y) values.


<!-- !! processed by numpydoc !! -->
