# Model File Format

## 1. Stoichiometric Matrix

A matrix element of the stoichiometric matrix Si,j denotes the number of molecules of metabolite i that participate in reaction j. All entries of this field are integer numbers. Only the non-zero matrix elements should be listed. The rest are given zero value by default.

```
SMATRIX  num_rows  num_cols
  row_1  col_1  stoichiometry
  row_2  col_2  stoichiometry
  row_3  col_3  stoichiometry
  ...
//
```

## 2. Flux Bounds

All flux bounds are set to the default_lower_bound and default_upper_bound values unless they are listed below. The reaction indices start with 1.

```
BOUNDS  default_lower_bound  default_upper_bound
  rxn_1  lb  ub
  rxn_2  lb  ub
  rxn_3  lb  ub
  ...
//
```

## 3a. Objective Reaction

The index of the objective reaction.

```
OBJECTIVE
  objective_reaction_index
//
```

Multiple objectives can be given in this block. In this case, the objective fluxes are solved and fixed in the order given.

By default, the flux through the objective is maximized. It can be minimized instead by placing a minus sign (-) before the objective reaction index.

For example, the following block would first minimize reaction 10, then maximize reaction 15:

```
OBJECTIVE
  -10 15
//
```

## 3b. Biomass Reaction

Optional. The index of the reaction that should be used to calculate the rate of biomass production. If this section is not included, the system will use the Objective reaction.

```
BIOMASS
  biomass_reaction_index
//
```

## 4. Metabolite Names

The metabolite names in the order consistent with the stoichiometric matrix.

```
METABOLITE_NAMES
  name_1
  name_2
  name_3
  ...
//
```

## 5. Reaction Names

The reaction names in the order consistent with the stoichiometric matrix.

```
REACTION_NAMES
  name_1
  name_2
  name_3
  ...
//
```

## 6. Exchange Reaction Indices

Indexes of all exchange reactions as listed under reaction_names. (Note that the first index is 1, not 0)

```
EXCHANGE_REACTIONS
  reaction_idx_1
  reaction_idx_2
  reaction_idx_3
  ...
//
```

## 7. Diffusion Constants

Deprecated: This won’t cause any errors if you leave it in, but it’s not used by COMETS

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block.

```
DIFFUSION_CONSTANTS  default_value
  exch_1  value
  exch_2  value
  exch_3  value
  ...
//
```

## 8. Alpha Values

Deprecated: This won’t cause any errors if you leave it in, but it’s not used by COMETS

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block.

```
ALPHA_VALUES  default_alpha
  exch_1  alpha_1
  exch_2  alpha_2
  exch_3  alpha_3
  ...
//
```

## 9. W Values

Deprecated: This won’t cause any errors if you leave it in, but it’s not used by COMETS

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block.

```
W_VALUES  default_W
  exch_1  W_1
  exch_2  W_2
  exch_3  W_3
  ...
//
```

## 10. Km Values

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block. The units for these values are mmol/cm3.

```
KM_VALUES  default_Km
  exch_1  Km_1
  exch_2  Km_2
  exch_3  Km_3
  ...
//
```

## 11. Vmax Values

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block. The units for these values are mmol/gCDW/hr. (gCDW being “grams cellular dry weight”).

```
VMAX_VALUES  default_Vmax
  exch_1  Vmax_1
  exch_2  Vmax_2
  exch_3  Vmax_3
  ...
//
```

## 12. Hill Coefficients

Deprecated. This block may be included to prevent breaking legacy layouts, but is no longer used.

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block.

```
HILL_COEFFICIENTS  default_Hill
  exch_1  Hill_1
  exch_2  Hill_2
  exch_3  Hill_3
  ...
//
```

## 13. Objective Style

Sets the objective style. The objective_style_string can be one of the values:

- MAXIMIZE_OBJECTIVE_FLUX
- MINIMIZE_OBJECTIVE_FLUX
- MAXIMIZE_TOTAL_FLUX
- MINIMIZE_TOTAL_FLUX
- MAX_OBJECTIVE_MAX_TOTAL
- MAX_OBJECTIVE_MIN_TOTAL
- MIN_OBJECTIVE_MAX_TOTAL
- MIN_OBJECTIVE_MIN_TOTAL

The default is MAXIMIZE_OBJECTIVE_FLUX. If combined with GUROBI optimizer only

MAXIMIZE_OBJECTIVE_FLUX and MAX_OBJECTIVE_MIN_TOTAL are functional.

```
OBJECTIVE_STYLE

objective_style_string

//
```

## 14. Optimizer

Only version 2.0.0 and up. Sets the optimizer. The optimizer_string can be one of the values:

- GUROBI
- GLPK

The default is GUROBI.

```
OPTIMIZER optimizer_string
```