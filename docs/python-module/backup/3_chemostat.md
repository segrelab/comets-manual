COMETS provides the functionality to run simulations in a chemostat. Here we
are going to simulate a chemostat with lactose as the sole carbon resource and
two strains of E. coli: one which is deficient in the ability to uptake
lactose, and one which is deficient in the ability to metabolize galactose.
We will use the ijo1366 model provided as part of cobrapy.

We will illustrate two ways in which chemostat simulations can be performed
using the Comets Python toolbox: 1) by manually assigning all the "parts," and
2) by using a helper function that is included.

Let's load that first, make a copy, then knock-out the relevant reactions.

```Python
import cobra
import cobra.test
import comets as c

E_no_galE = cobra.test.create_test_model('ecoli') # this model will have galE KO'd
E_no_LCTStex = E_no_galE.copy() # here we'll KO lactose uptake
```

Note that the first model had galE knocked out to prevent metabolism of
galactose, which will cause it to be secreted. The second model has a reaction
knocked out instead of a gene, as lactose transport to the periplasm can be
accomplished with multiple genes, and it is simpler to just knockout the
reaction itself.

We can test that the knockouts perform as expected by trying to grow them in
media containing lactose and galactose:

```Py
medium = E_no_galE.medium
medium["EX_glc_e"] = 0.
medium["EX_lcts_e"] = 1.
medium["EX_gal_e"] = 1.

# medium:
{'EX_ca2_e': 1000.0,
'EX_cbl1_e': 0.01,
'EX_cl_e': 1000.0,
'EX_co2_e': 1000.0,
'EX_cobalt2_e': 1000.0,
'EX_cu2_e': 1000.0,
'EX_fe2_e': 1000.0,
'EX_fe3_e': 1000.0,
'EX_gal_e': 1.0,
'EX_h_e': 1000.0,
'EX_h2o_e': 1000.0,
'EX_k_e': 1000.0,
'EX_lcts_e': 1.0,
'EX_mg2_e': 1000.0,
'EX_mn2_e': 1000.0,
'EX_mobd_e': 1000.0,
'EX_na1_e': 1000.0,
'EX_nh4_e': 1000.0,
'EX_ni2_e': 1000.0,
'EX_o2_e': 1000.0,
'EX_pi_e': 1000.0,
'EX_sel_e': 1000.0,
'EX_slnt_e': 1000.0,
'EX_so4_e': 1000.0,
'EX_tungs_e': 1000.0,
'EX_zn2_e': 1000.0,
'EX_glc_e': 0.0}

E_no_galE.medium = medium
E_no_LCTStex.medium = medium

# running the below functions shows growth in each case, but only using
# lactose in the first case and only using galactose in the second case.
# Additionally, we see that E_no_galE secretes galactose during growth on lactose.
E_no_galE.summary()
E_no_LCTStex.summary()
```

Now we are ready to setup a COMETS simulation. Let's intend that the medium
shown above is the reservoir (except that we will remove the galactose first),
and the input rate and output rate are 10% / hr. Let's set that parameter and
make our COMETS models.

```Py
# chemostat parameters:
dilution_rate = 0.1 # / hr
initial_pop = 1e-3

# Make the models into COMETS models:
E_no_galE.id = "galE_KO"
E_no_LCTStex.id = "LCTStex_KO"

E_no_galE = c.model(E_no_galE)
E_no_galE.initial_pop = [0, 0, initial_pop]

# next line allows medium to constrain uptake
E_no_galE.reactions.loc[E_no_galE.reactions.EXCH, 'LB'] = -1000

E_no_LCTStex = c.model(E_no_LCTStex)
E_no_LCTStex.initial_pop = [0, 0, initial_pop]

# next line allows medium to constrain uptake
E_no_LCTStex.reactions.loc[E_no_LCTStex.reactions.EXCH, 'LB'] = -1000
Now we are going to use the manual method for making a chemostat. Recall that
Cobra mediums are set using exchange reaction ids, whereas COMETS media are
set using metabolite ids. We can easily take care of this with a dictionary
comprehension. Here we do that, then generate a layout, and add to it the
medium.

# setup layout
comets_media = {key[3:] : value for key, value in medium.items() if key != "EX_gal_e"}
layout = c.layout([E_no_galE, E_no_LCTStex])

# set initial media
for key, value in comets_media.items():
layout.set_specific_metabolite(key, value)
```

The input of fresh media from the reservoir into the simulation is done using
media_refresh. Metabolites with a media_refresh value are replenished at the
specified amount per-hour. Since we are diluting at 0.1 per hour, we multiply
the reservoir concentration by this rate.

```Py
# set metabolite drip
for key, value in comets_media.items():
layout.set_specific_refresh(key, value * dilution_rate)
```

The rest of the chemostat—the output flow—is setup in the parameters using the
metaboliteDilutionRate and deathRate parameters, which should be set equal to
our dilution rate:

```Py
params = c.params()
params.all_params['deathRate'] = dilution_rate
params.all_params['metaboliteDilutionRate'] = dilution_rate
```

Let's also change a few other parameters.

```Py
params.all_params['timeStep'] = 0.1
params.all_params["maxSpaceBiomass"] = 10
params.all_params['maxCycles'] = 2000
```

Finally, lets keep track of just a few key metabolites: lactose and galactose. We do this using the specificMedia log, and choosing the metabolites with a comma-separated list with no spaces.

```Py
params.all_params['writeSpecificMediaLog'] = True
params.all_params['specificMediaLogRate'] = 1
params.all_params['specificMedia'] = 'lcts_e,gal_e'
```

Now we can generate a COMETS simulation object, run it, then examine the biomass and metabolites.

```Py
sim = c.comets(layout, params)
sim.run()
sim.total_biomass.plot(x = "cycle")
# in the next line, we exclude the x and y columns, as this was not a spatial simulation
sim.specific_media.ix[:, sim.specific_media.columns.difference(["x","y"])].plot(x = "cycle")
```

We see that both species survive, with LCTStex_KO cross-feeding galactose. The
metabolites, as is typical in chemostat, are in very low concentrations once
equilibrium is reached.

The above code required setting chemostat parameters in multiple places. We
offer this functionality so that researchers can create complex setups that
may, for example, have different initial concentrations than reservoir
concentrations, and different inflow rates than outflow rates. However, we
expect most chemostat simulations will function like above, where a single
dilution parameter dictates the behavior of the system. For this, we have
made a helper function which generates a layout and parameters file with the
correct setup:

```Py
# here the first argument is a list of comets models, the second the reservoir media, and the third is the dilution rate
layout, params = c.chemostat([E_no_galE, E_no_LCTStex], comets_media, dilution_rate)

# we can then adjust other parameters as desired, and run.
params.all_params['timeStep'] = 0.1
params.all_params["maxSpaceBiomass"] = 10
params.all_params['maxCycles'] = 2000
params.all_params['writeSpecificMediaLog'] = True
params.all_params['specificMediaLogRate'] = 1
params.all_params['specificMedia'] = 'lcts_e,gal_e'

sim = c.comets(layout, params)
sim.run()
sim.total_biomass.plot(x = "cycle")
sim.specific_media.ix[:, sim.specific_media.columns.difference(["x","y"])].plot(x = "cycle")
```
