One of COMETS features is the ability to include mutations in models during a
simulation, thereby simulating evolution. At the moment, mutations consist in
either reaction deletions or additions.

In this example, we simulate an experimental evolution of *E. coli* in batch
culure, with serial transfers every 24 hours.

We start as usual by loading the toolbox and setting up the layout. We will use
a well mixed environment with a glucose minimal media.

```Python
import comets as c
import os
import pandas as pd
import matplotlib.pyplot as plt

# generate layout
world = c.layout()
world.set_specific_metabolite('glc__D_e', 0.01)
```

AddWe also add trace metabolites (ions, metals etc) in unlimited amounts. For
this, we set the static flag to True and the static value to 1000mM.

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

We load our model and add it to the layout

```Python
# load model
wt = c.model('test_models/UiJO1366_nonTrimmed.cmd')
wt.initial_pop = [0, 0, 5e-8]
world.add_model(wt)
```

We now create a `params` object, and modify the needed parameters. For this
small example, we will perform 10 transfers every 24h. We will set the mutation
rate to $10^6$ for deletions. We also set a cost term for genome size of 5e-8,
so that deletion of useless mutations can be slightly beneficial due to
reduction in genome maitenance.

```Python
# .. load parameters and layout from file
evo_params = c.params()

#simulate 10 serial transfers of 24h each (timeStep = 0.1)
evo_params.all_params['timeStep'] = 0.1
evo_params.all_params['maxCycles'] = 2400
evo_params.all_params['batchDilution'] = True
evo_params.all_params['dilFactor'] = 0.01
evo_params.all_params['dilTime'] = 24

evo_params.all_params['evolution'] = True
evo_params.all_params['mutRate'] = 1e-5
evo_params.all_params['cellSize'] = 1e-9
#evo_params.all_params['minCellBiomass'] = 2.5e-13

evo_params.all_params['costlyGenome'] = True
evo_params.all_params['geneFractionalCost'] = 5e-8

evo_params.all_params['BiomassLogRate'] = 1 #2400
```

Finally, we create the COMETS object using the above layout and parameters, and
run the simulation.

```Python
# create comets object from the loaded parameters and layout
evo_simulation = c.comets(world, evo_params)
# run comets simulation
evo_simulation.run()
```

We can visualize the evolution over time using the `biomass` output data frame.

```Python
fig, ax = plt.subplots(figsize=(15, 5))

for key, grp in evo_simulation.biomass.groupby(['species']):
    ax = grp.plot(ax=ax, kind='line', x='Cycle', y='biomass')
ax.get_legend().remove()
plt.yscale('log')
plt.show()
```

![png](../img/evolution_1.png)


It is also helpful to the analyze the `genotype` data frame output. This table
contains all the mutants that appear during the simulation, in three columns:
The ancestor, the mutation, and the name of the resulting genotype, which is
assigned a random hash as ID (or "Species"). Note that the first genotype in
the list is the loaded model, so it has "no ancestor" and "no mutation". 

```Python
Evo_simulation.genotypes
```
|   | Ancestor                | Mutation | Species                              |
|---|-------------------------|----------|--------------------------------------|
| 0 | NO_ANCESTOR             | NO_MUT   | UiJO1366_nonTrimmed.cmd              |
| 1 | UiJO1366_nonTrimmed.cmd | add_5510 | e242b9ca-e8ea-46ab-aaeb-09b24b690d6c |
| 2 | UiJO1366_nonTrimmed.cmd | del_1214 | 4c1d16f3-976e-446b-87d2-3126d8a4310f |
| 3 | UiJO1366_nonTrimmed.cmd | add_2944 | 46ffd601-3ca7-4d63-93ee-88c7dee6ea03 |
| 4 | UiJO1366_nonTrimmed.cmd | add_5172 | 9cd5b1a8-5768-4e1c-8c55-2f0271f9ee36 |
| 5 | UiJO1366_nonTrimmed.cmd | del_1709 | fb39194a-9892-4942-b817-5261b2bd0da6 |
| . | ... | ... | ... |
