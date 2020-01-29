Competition experiments are frequently performed in the laboratory to assay,
for example, the fitness of a mutant in competition to the wild-type. Here, we
simulate one such experiment involving E. coli and a nonessential but
deleterious mutation involving the deletion of the triose phosphate isomerase
reaction from glycolysis.

We start by creating a layout ("world") and setting the initial nutrient supply.

```Python
# Start by loading required packages, including the COMETS toolbox
import comets as c
import pandas as pd

# create an empty layout
world = c.layout()

# Add glucose and remove o2
world.set_specific_metabolite('glc__D_e', 0.01)
```

We will now add the typical trace metabolites to the layout we generated
(including oxygen). These metabolites will be considered as `static` by the
simulation. The `static` option implies that a metabolite is set to a given
value and kept constant throughout the simulation. This is useful, for example,
for media components that we want to be always in excess.

```Python
# Add typical trace metabolites and oxygen coli as static
trace_metabolites = ['ca2_e',
                     'cl_e',
                     'cobalt2_e',
                     'cu2_e',
                     'fe2_e',
                     'fe3_e',
                     'h_e',
                     'k_e',
                     'h2o_e',
                     'mg2_e',
                     'mn2_e',
                     'mobd_e',
                     'na1_e',
                     'ni2_e',
                     'nh4_e',
                     'o2_e',
                     'pi_e',
                     'so4_e',
                     'zn2_e']

for met in trace_metabolites:
    newrow = {'metabolite': met,
              'g_refresh': 0,
              'g_static': 1,
              'g_static_val': 1000,
              'init_amount': 1000,
              'diff_c': world.default_diff_c}

    newrow = pd.DataFrame([newrow], columns=newrow.keys())
    world.media = pd.concat([world.media, newrow], axis=0, sort=False)
```

Now, we load the *E. coli* iJO1366 model, and create also a KO mutant for
the triose phosphate isomerase reaction. We do this by setting both upper and
lower bounds to zero. We then add both models to our `layout`.

```Python
# load the wild-type model
wt = c.model('test_models/iJO1366.xml')

# load the wild-type model and construct the mutant
mut = c.model('test_models/iJO1366.xml')
mut.change_bounds('TPI', 0,0)
mut.id = 'TPI_iJO1366'

# set the initial biomasses for both models
wt.initial_pop = [0, 0, 5e-8]
mut.initial_pop = [0, 0, 5e-8]

# add the models to the layout
world.add_model(wt)
world.add_model(mut)
```

We need also to create a parameters object and modify needed parameters. In
this case, we will modify only the number of cycles the simulation runs.

```Python
comp_params = c.params()
comp_params.all_params['maxCycles'] = 240
```

Finally, we pass all of the above to a `comets` object, and run the competition
assay.

```Python
comp_assay = c.comets(world, comp_params)
comp_assay.run()
```
Now, we can plot the biomasses of these two genotypes in co-culture.

```Python
biomass = comp_assay.total_biomass
biomass['t'] = biomass['cycle'] * comp_assay.parameters.all_params['timeStep']

import matplotlib.pyplot as plt
myplot = biomass.drop(columns=['cycle']).plot(x = 't')
```

![png](/img/competition_1.png)


We can also compute the competitive fitness of the mutant with respect to the
wild type, as the ratio of the biomass increase of the mutant divided by that
of the wild-type:

```Python
cfit = (biomass.loc[biomass['t'] == 24, 'TPI_iJO1366'].iloc[0]/biomass.loc[biomass['t'] == 0, 'TPI_iJO1366'].iloc[0])/(biomass.loc[biomass['t'] == 24, 'iJO1366'].iloc[0]/biomass.loc[biomass['t'] == 0, 'iJO1366'].iloc[0])
```

# Serial transfers and competitive exclusion
To illustrate the serial transfer capabilities of COMETS, we will now simulate
a serial transfer competition between these two genotypes.

We will just create a different set of parameters, increasing the total
simulation time and including batch transfers of 1% every 24h, but we will use
the same "world" `layout` as before.

!!! note
    When performing serial transfers, the biomass is discretized according to
    the `cellSize` parameter. This allows for stochastic sampling when the
    bottleneck is applied, which can be important if some species are present
    at low density. The value of `cellSize` must be always bigger than the
    minimal biomass allowed for a model in a cell (defined by the
    `minCellBiomass`parameter).


```Python
serial_params = c.params()
serial_params.all_params['maxCycles'] = 240*25 # simulate 4 serial transfers of 24h each
serial_params.all_params['batchDilution'] = True
serial_params.all_params['dilFactor'] = 0.01
serial_params.all_params['dilTime'] = 24
serial_params.all_params['cellSize'] = 1e-9
```

We now run the simulation

```Python
serial_expt = c.comets(world, serial_params)
serial_expt.run()
```

And plot the biomasses of the two species during an experiment with 25 transfers every 24h

```Py
biomass = serial_expt.total_biomass
biomass['transfer'] = biomass['cycle'] * comp_assay.parameters.all_params['timeStep']/24

myplot = biomass.drop(columns=['cycle', 't']).plot(x = 'transfer')
```
![png](/img/competition_2.png)
