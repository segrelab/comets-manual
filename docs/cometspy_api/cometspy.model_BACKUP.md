# cometspy.model module

<!-- !! processed by numpydoc !! -->

### class cometspy.model.model(model: cobra.core.model.Model = None)
Bases: `object`

a COMETS metabolic model to use in a layout

A model contains information including the stoichiometric matrix,
reaction boundaries and Michaelis-Menten parameters, founder populations,
and various attributes related to motion in space.

A model is usually initiated by supplying a cobrapy model, and to be used
in a COMETS simulation must minimally be given an initial population:


* **Parameters**

    **model**

        Either a cobrapy Model, a path to a .cmd COMETS model, a path to a
        cobra sbml model which could be loaded with cobra.io.read_sbml_model()
        or None.


### Examples

```python
>>> import cobra.test
>>> import cometspy as c
>>> ecoli = cobra.test.create_test_model("ecoli")
>>> model = c.model(ecoli)
>>> model.initial_pop = [0, 0, 1.e-12] # puts 1.e-12 gDW of biomass at 0,0
>>> print(model.id)
iJO1366
```


* **Attributes**

    **initial_pop**

        list of lists of spatial founder pops. e.g.[[x, y, grams], [x,y,grams]]

    **id**

        the name of the model. should be unique.

    **reactions**

        DataFrame containing reaction information including boundaries.

    **smat**

        DataFrame containing the stoichiometric matrix

    **metabolites**

        single-column DataFrame containing the names of the metabolites

    **signals**

        DataFrame on reactions whose bounds depend on metabolite concentrations

    **default_vmax**

        default uptake vmax in mmol / gDW /hr for Michaelis-Menten kinetics

    **default_km**

        default half-max metabolite concentration (M) for M-M kinetics

    **default_hill**

        default hill coefficient for hill-like uptake kinetics

    **default_bounds**

        list of two floats that are the default lower and upper reaction bounds

    **obj_style**

        one of MAXIMIZE_OBJECTIVE_FLUX (fba) or MAX_OBJECTIVE_MIN_TOTAL (pfba)

    **optimizer**

        one of “GUROBI” or “GLPK”. not all functionality works with GLPK


### Methods

| `add_convection_parameters`([packedDensity, …])

 | sets the parameters for biomass spread to use convection

 |
| `add_light`(reaction, abs_coefficient, abs_base)

  | Causes a reaction to function in response to light

                    |
| `add_neutral_drift_parameter`(neutralDriftSigma)

  | sets the neutralDriftSigma parameter

                                  |
| `add_noise_variance_parameter`(noiseVariance)

     | sets the noise variance parameter

                                     |
| `add_nonlinear_diffusion_parameters`([zero, …])

   | sets the model to use non-linear diffusion biomass spread.

            |
| `add_signal`(rxn_num, exch_ind, bound, …)

         | adds a signal to the reaction rxn_num that changes bounds

             |
| `change_bounds`(reaction, lower_bound, upper_bound)

 | changes the bounds of the specified reaction

                          |
| `change_hill`(reaction, hill)

                       | changes the hill coefficient of the specified reaction.

               |
| `change_km`(reaction, km)

                           | changes the km of the specified reaction.

                             |
| `change_vmax`(reaction, vmax)

                       | changes the vmax of the specified reaction.

                           |
| `delete_comets_model`([working_dir])

                | deletes a file version of this model if it exists.

                    |
| `ensure_sinks_are_not_exchanges`([suffix])

          | set exchange reactions ending in suffix (default = ‘_c’) to sink

      |
| `get_bounds`(reaction)

                              | returns a tuple (lb, ub) for the specified reaction

                   |
| `get_exchange_metabolites`()

                        | returns a list of the names of the exchange metabolites

               |
| `get_reaction_names`()

                              | returns a list of reaction names

                                      |
| `load_cobra_model`(curr_m)

                          | creates the COMETS model from the supplied cobra model

                |
| `open_exchanges`([lower_bound, upper_bound])

        | sets all exchange boundaries to the specifed values

                   |
| `read_cobra_model`(path)

                            | reads a cobra model from a file and loads it into this model

          |
| `read_comets_model`(path)

                           | an alternative way to create a COMETS model by reading from a file

    |
| `write_comets_model`([working_dir])

                 | writes the COMETS model object to a file

                              |
<!-- !! processed by numpydoc !! -->

#### add_convection_parameters(packedDensity: float = 1.0, elasticModulus: float = 1.0, frictionConstant: float = 1.0, convDiffConstant: float = 1.0)
sets the parameters for biomass spread to use convection

This also requires one set the biomassMotionStyle to 
‘Convection 2D’ in the comets.params object (see Example)


* **Parameters**

    **packedDensity**

    **elasticModulus**

    **frictionConstant**

    **convDiffConstant**


<!-- !! processed by numpydoc !! -->

#### add_light(reaction: str, abs_coefficient: float, abs_base: float)
Causes a reaction to function in response to light


* **Parameters**

    **reaction**

        the name of the reaction affected

    **abs_coefficient**

        the absorption coefficient

    **abs_base**

        absorption baseline


<!-- !! processed by numpydoc !! -->

#### add_neutral_drift_parameter(neutralDriftSigma)
sets the neutralDriftSigma parameter

<!-- !! processed by numpydoc !! -->

#### add_noise_variance_parameter(noiseVariance: float)
sets the noise variance parameter


* **Parameters**

    **noiseVariance**


<!-- !! processed by numpydoc !! -->

#### add_nonlinear_diffusion_parameters(zero: float = 1.0, n: float = 1.0, exponent: float = 1.0, hilln: float = 10.0, hillk: float = 0.9)
sets the model to use non-linear diffusion biomass spread.

This also requires one set the biomassMotionStyle to 
‘ConvNonlin Diffusion 2D’ in the comets.params object (see Example)


* **Parameters**

    **zero**

    **n**

    **exponent**

    **hilln**

    **hillk**


<!-- !! processed by numpydoc !! -->

#### add_signal(rxn_num, exch_ind, bound, function, parms)
adds a signal to the reaction rxn_num that changes bounds


* **Parameters**

    **rxn_num**

        reaction affected (int) or whether the signal causes death ‘death’

    **exch_id**

        the number of the exchange reaction. see model.metabolites

    **bound**

        specifies whether the upper or lower bound is affected

    **function**

        specifies the function relating the metabolite conc. to the bound

    **parms**

        a list of floats for the function


<!-- !! processed by numpydoc !! -->

#### change_bounds(reaction: str, lower_bound: float, upper_bound: float)
changes the bounds of the specified reaction


* **Parameters**

    **reaction**

        the name of the reaction

    **lower_bound**

        the new value of the reaction’s lower bound in mmol / gDW / hr

    **upper_bound**

        the new value of the reaction’s upper bound in mmol / gDW / hr


<!-- !! processed by numpydoc !! -->

#### change_hill(reaction, hill)
changes the hill coefficient of the specified reaction.


* **Parameters**

    **reaction**

        the name of the reaction

    **hill**

        the new hill coef. for the reaction, for Hill-like kinetics


<!-- !! processed by numpydoc !! -->

#### change_km(reaction, km)
changes the km of the specified reaction.


* **Parameters**

    **reaction**

        the name of the reaction

    **km**

        the new km value for the reaction, for Monod (Michaelis-Menten)
        kinetics


<!-- !! processed by numpydoc !! -->

#### change_vmax(reaction: str, vmax: float)
changes the vmax of the specified reaction.


* **Parameters**

    **reaction**

        the name of the reaction

    **vmax**

        the new vmax value for the reaction, for Monod (Michaelis-Menten)
        kinetics


<!-- !! processed by numpydoc !! -->

#### delete_comets_model(working_dir=None)
deletes a file version of this model if it exists.


* **Parameters**

    **working_dir**

        the directory where the comets model file exists


<!-- !! processed by numpydoc !! -->

#### ensure_sinks_are_not_exchanges(suffix: str = '_c')
set exchange reactions ending in suffix (default = ‘_c’) to sink


* **Parameters**

    **suffix**

        the suffix to look for in exchange reactions. default = ‘_c’


<!-- !! processed by numpydoc !! -->

#### get_bounds(reaction: str)
returns a tuple (lb, ub) for the specified reaction


* **Parameters**

    **reaction**

        the name of the reaction


<!-- !! processed by numpydoc !! -->

#### get_exchange_metabolites()
returns a list of the names of the exchange metabolites

<!-- !! processed by numpydoc !! -->

#### get_reaction_names()
returns a list of reaction names

<!-- !! processed by numpydoc !! -->

#### load_cobra_model(curr_m: cobra.core.model.Model)
creates the COMETS model from the supplied cobra model

This is usually used internally when creating a COMETS model.


* **Parameters**

    **curr_m**

        the cobra model object to be converted to the COMETS model object


<!-- !! processed by numpydoc !! -->

#### open_exchanges(lower_bound: float = - 1000.0, upper_bound: float = 1000.0)
sets all exchange boundaries to the specifed values

This method is useful when first making a COMETS model so that medium
definitions are not carried over from cobra into COMETS.


* **Parameters**

    **lower_bound**

        default = -1000.   in units of mmol / gDW / hr

    **upper_bound**

        default = 1000.    in units of mmol / gDW / hr


<!-- !! processed by numpydoc !! -->

#### read_cobra_model(path: str)
reads a cobra model from a file and loads it into this model

This is an alternative way to initialize a COMETS model, by supplying
it with the path to a cobra model that can be read using
cobra.io.read_sbml_model.


* **Parameters**

    **path**

        path to a cobra model in sbml format


<!-- !! processed by numpydoc !! -->

#### read_comets_model(path: str)
an alternative way to create a COMETS model by reading from a file

This allows a user to load previously-saved COMETS models. The 
contents populate this object’s attributes.


* **Parameters**

    **path**

        the path to the COMETS model file


<!-- !! processed by numpydoc !! -->

#### write_comets_model(working_dir: str = None)
writes the COMETS model object to a file

This writes the current model object in COMETS format to file, either
in the current working directory or in the directory specified by
the optional parameter working_dir.  It is mostly used by the comets
object to run simulations, though a user may with to write these to 
examine directly or to save.


* **Parameters**

    **working_dir**

        a directory path to put COMETS files.  for example “./data_files/”


<!-- !! processed by numpydoc !! -->
