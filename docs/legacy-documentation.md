# Quick Start

COMETS is started by running one of the the comets_win_x64 or comets_win_x32 batch files in Windows or running the ./comets script in Linux. Once it is started, a “Welcome to COMETS!” window shows up. Click on the “Load layout file” button and navigate to the models directory. Choose a layout file to load. Once the layout is loaded the layout will show up on the screen. To change the default parameters go to the Edit tab and choose Edit Parameters. When done editing the parameters you can run a simulation by going to the Simulation tab and choosing Start Simulation. The Simulation panel will show up in the main frame. To start a simulation click on the Run/Pause Simulation button.

# System Requirements

For best performance, COMETS requires [Java JRE 1.8](https://java.com/en/download/) or above and [Gurobi](http://www.gurobi.com/) 7.5 or higher.

# Install and Run Comets

In order to run COMETS you must have Java installed on your system. Java can be downloaded and installed from [here](http://www.oracle.com/technetwork/java/javase/downloads/index.html). Make sure that you have the Java bin directory in your [PATH system variable](http://www.java.com/en/download/help/path.xml).

### Older Versions

These instructions are relevant for COMETS version 2.0.0 and up. The instruction for older versions are [here](http://www.bu.edu/segrelab/install-and-run-comets-2/).

### Installing on Windows

COMETS for Windows is distributed as an archived .zip file. To extract it double-click on it or right-click and choose Extract All… This will create a directory COMETS_X.Y.Z_win\comets_X.Y.Z, where X.Y.Z is the version number.

The directory contains the INSTALL file. Read this file and follow the instructions to install COMETS.

### Installing on Mac

COMETS for Linux is distributed as an archived COMETS_X.Y.Z_mac.tar.gz file, where X.Y.Z is the version number. To extract it, run it in a terminal:

$tar -zxvf COMETS_X.Y.Z_mac.tar.gz

where X.Y.Z are replaced with the version numbers.

This will create a directory COMETS_X.Y.Z_mac/comets_X.Y.Z. It contains INSTALL file. To install COMETS read the INSTALL file.

### Installing on Linux

COMETS for Linux is distributed as an archived COMETS_X.Y.Z.tar.gz file, where X.Y.Z is the version number. To extract it, run it in a terminal:

$tar -zxvf COMETS_X.Y.Z.tar.gz

where X.Y.Z are replaced with the version numbers.

This will create a directory COMETS_X.Y.Z/comets_X.Y.Z. It contains INSTALL file. To install COMETS read the INSTALL file.

## Running COMETS

The instructions for running COMETS are in the RUNNING_COMETS file in the comets_X.Y.Z directory.

The comets_X.Y.Z/layouts_and_models directory contains three examples of layouts and two models. The model_CSP_petri_dish_circles is an example of a two dimensional layout with a toy model model_CSP. The model_CSP_3D_layout is a three dimensional layout with the same toy model.

The Ecoli_colony_layout is a layout for growth of an E.coli colony in two dimensions. The corresponding model is in the EC_ijo1366_model file. A detailed description of running this layout and model is [here](http://www.bu.edu/segrelab/comets-e-coli-simulation-example/).

# COMETS E. coli Simulation Example

TODO: copy from <https://www.bu.edu/segrelab/comets-e-coli-simulation-example/>

# Layout File Format

The data blocks in this file are mostly nested within each other. The model file block encapsulates the model world and initial population blocks. The rest of the blocks are listed in the model world block.

##  1. Model Names

This is a single line block, NOT followed by // on a line below, but at the end of the file. This must be the first line of the layout file. It is followed by the name of at least one FBA model file.


model_file model_file_name1 model_file_name2 ... model_file_name_N



## 2. Model World

This is a single line block, NOT followed by // on a line below, but after several internal data blocks. This must be the first block inside the model names block. It simply opens up the data block containing the world information. Optionally, it can be followed by the name of a file containing global media information (note that this is deprecated! Global media information should be included in the layout file under the world media block).


model_world

### 2a. Grid Size

This is required to be the first block under the model world line. It doesn’t have a closing //, and only describes the width and height of the simulation grid.

grid_size  width  height

### 2b. World Media

The world media block contains global information of the quantity of all extracellular metabolites in the simulation. Each row in the block has two elements – the name of an extracellular metabolite (must be exactly as found in the model file) followed by the quantity in millimoles of that metabolite initialized in every block in the simulation.

This block must come before the media refresh, static media, and diffusion constants blocks.

world_media
  metab_1  quantity_1
  metab_2  quantity_2
  metab_3  quantity_3
  .
  .
  .
  metab_N  quantity_N
//

### 2c. Diffusion Constants

The diffusion constants block contains diffusion constant information for the extracellular metabolites in the simulation. The block begins with a global default value on the same line as the opening tag. Each row in the block, then, has two elements – the index of the metabolite in question, in the same order as in the world_media block above (starting from 0); and a value for the diffusion constant if it differs from the default.

Units for these values are cm2 /s. 


diffusion_constants    default_value
  index_1  diff_const_1
  index_2  diff_const_2
  index_3  diff_const_3
  .
  .
  .
//

###  2d. Media Refresh

This block describes how much each medium component should be refreshed on each simulation cycle in any given grid space. The opening line contains global refresh values for each medium component, in the same order as presented in the world media block.


Each proceeding row contains the amount of media to refresh for each grid space where there is a nonzero refresh value. That is, the only rows that should be present in this block should contain some amount of medium to be refreshed.


media_refresh  global_amt_1  global_amt_2  ...  global_amt_N
  row_1  col_1  amt_1  amt_2  amt_3  ...  amt_N
  row_2  col_2  amt_1  amt_2  amt_3  ...  amt_N
  row_3  col_3  amt_1  amt_2  amt_3  ...  amt_N
  .
  .
  .
  row_M  col_M  amt_1  amt_2  amt_3  ...  amt_N
//

### 2e. Static Media

Like the media refresh block, this describes how the media in each space in the grid should be altered during the simulation. However, this block describes which media components should remain at a static value. That is, those media whose quantities should not change due to the effects of the simulation. This might represent a constant sink or source of a number of metabolites.

This block contains pairs of data elements. Each pair starts with a binary (0 or 1) element, denoting whether or not to use that pair as a static element, and a quantity to keep that element static at. This lets one build a static media set where only a few medium elements are maintained as static, while keeping the file structure similar to the media refresh block above.
For example:


static_media  0  0  1  0  1  2  0  0
//



Breaks into 4 pairs [0 0], [1 0], [1 2], [0 0]. Only components 2 and 3 have a 1 as their first element, so those medium components are kept static. Component 2 remains at 0, while component 3 stays at 2. Note that in this example, there are no exceptions, so this applies throughout the simulation. In general, it is done like this:


static_media  use_1  amt_1  use_2  amt_2  use_3  amt_3  ...  use_N  amt_N
  row_1  col_1  use_1  amt_1  use_2  amt_2  ...  use_N  amt_N
  row_2  col_2  use_1  amt_1  use_2  amt_2  ...  use_N  amt_N
  row_3  col_3  use_1  amt_1  use_2  amt_2  ...  use_N  amt_N
  .
  .
  .
  row_M  col_M  use_1  amt_1  use_2  amt_2  ...  use_N  amt_N
//

### 2f. Barrier Coordinates

Simple enough, this block describes those grid coordinates that contain barriers (i.e., no media, biomass, or diffusion can occur here).


barrier
  row_1  col_1
  row_2  col_2
  row_3  col_3
  .
  .
  .
  row_N  col_N
//

## 3. Initial Population Biomass

This final block describes the initial biomass population in the layout, and has many options.

The units for the biomass amounts are grams (for example, 1e-6 denotes one microgram of the corresponding species).

In the default biomass layout style, there is just initial_pop left alone as the header, followed by one row for each grid space containing biomass. There must be one quantity for each metabolic model in the system.


initial_pop
  row_1  col_1  biomass_1  biomass_2  ...  biomass_N
  row_2  col_2  biomass_1  biomass_2  ...  biomass_N
  row_3  col_3  biomass_1  biomass_2  ...  biomass_N
  .
  .
  .
  row_M  col_M  biomass_1  biomass_2  ...  biomass_N
//


The next set of options modify the initial_pop line itself, and contain no rows in the block, though the // on the line below must be present.

initial_pop  random  N1  biomass_1  N2  biomass_2 ...
//

This will randomly assign Ni spaces to have biomass_i amount of biomass from model i. If biomass from different species is allowed to overlap, then that is allowed to happen in this case, but if different species is not allowed to overlap, then that rule is upheld.

initial_pop  random_rect  X  Y  W  H  N1  biomass_1  N2  biomass_2 ...
//

As above, but the randomly assigned biomass spots are confined to a rectangle with its upper-left corner at grid space (X,Y), with W grid spaces wide, and H spaces tall.

initial_pop  filled  biomass_1  biomass_2 ...
//

This will completely fill the grid with the given amount of biomass from each species.

initial_pop  filled_rect  X  Y  W  H  biomass_1  biomass_2 ...
//

This will completely fill the rectangle (see above) with biomass from each species.

initial_pop  square  N1  biomass_1  N2  biomass_2 ...
//

This will fill squares with radius Ni at the center of the grid with biomass_i from species i.

## 4. Parameters

This block contains parameters initially loaded with the file. A table of accepted parameters and their default values is [provided](https://docs.google.com/document/d/19GtXv3ej147Q6a4ftW76KMCTPzC3kO3Cd0B4H68Jf6Y/edit#heading=h.rbzwd69jdafl) at the end of this document. Here’s the structure of the block:

parameters
  param_1 = value_1
  param_2 = value_2
  .
  .
  .
  param_N = value_N
//

##  Sample Layout File


model_file ec_iAF1260.cmf
  model_world
​    grid_size 100 100
​    world_media
​      M1  0
​      M2  100
​      M3  100
​      M4  0
​      M5  50
​    //
​    diffusion_constants  1e-6
​      0   1e-5
​      1   5e-6
​      4   2.5e-6
​    //
​    media_refresh 0 0 0 0 0
​      25 20 0 10 10 0 5
​      30 35 0 15 15 0 0
​    //
​    static_media 1 0 0 0 0 0 0 0 0 0
​      10 10 0 0 1 10 1 20 0 0 0 0
​    //
  //
  initial_pop random 20 1
  //
  parameters
​    timeStep = 1
​    numRunThreads = 10
​    allowCellOverlap = false
  //
//

# Model File Format

## 1. Stoichiometric Matrix

A matrix element of the stoichiometric matrix Si,j denotes the number of molecules of metabolite i that participate in reaction j. All entries of this field are integer numbers. Only the non-zero matrix elements should be listed. The rest are given zero value by default.

SMATRIX  num_rows  num_cols

  row_1  col_1  stoichiometry

  row_2  col_2  stoichiometry

  row_3  col_3  stoichiometry

  .

  .

  .

//

## 2. Flux Bounds

All flux bounds are set to the default_lower_bound and default_upper_bound values unless they are listed below. The reaction indices start with 1.

BOUNDS  default_lower_bound  default_upper_bound

  rxn_1  lb  ub

  rxn_2  lb  ub

  rxn_3  lb  ub

  .

  .

  .

//

## 3a. Objective Reaction

The index of the objective reaction.

OBJECTIVE

  objective_reaction_index

//

## 3b. Biomass Reaction

Optional. The index of the reaction that should be used to calculate the rate of biomass production. If this section is not included, the system will use the Objective reaction.

BIOMASS

  biomass_reaction_index

//

## 4. Metabolite Names

The metabolite names in the order consistent with the stoichiometric matrix.

METABOLITE_NAMES

  name_1

  name_2

  name_3

  .

  .

  .

//

## 5. Reaction Names

The reaction names in the order consistent with the stoichiometric matrix.

REACTION_NAMES

  name_1

  name_2

  name_3

  .

  .

  .

//

## 6. Exchange Reaction Indices

Exchange reaction indices as listed under reaction_names, starting with 1.

EXCHANGE_REACTIONS

  reaction_idx_1

  reaction_idx_2

  reaction_idx_3

  .

  .

  .

//

## 7. Diffusion Constants

Deprecated: This won’t cause any errors if you leave it in, but it’s not used by COMETS

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block.

DIFFUSION_CONSTANTS  default_value

  exch_1  value

  exch_2  value

  exch_3  value

  .

  .

  .

//

## 8. Alpha Values

Deprecated: This won’t cause any errors if you leave it in, but it’s not used by COMETS

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block.

ALPHA_VALUES  default_alpha

  exch_1  alpha_1

  exch_2  alpha_2

  exch_3  alpha_3

  .

  .

  .

//

## 9. W Values

Deprecated: This won’t cause any errors if you leave it in, but it’s not used by COMETS

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block.

W_VALUES  default_W

  exch_1  W_1

  exch_2  W_2

  exch_3  W_3

  .

  .

  .

//

## 10. Km Values

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block. The units for these values are mmol/cm3.

KM_VALUES  default_Km

  exch_1  Km_1

  exch_2  Km_2

  exch_3  Km_3

  .

  .

  .

//

## 11. Vmax Values

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block. The units for these values are mmol/gCDW/hr. (gCDW being “grams cellular dry weight”).

VMAX_VALUES  default_Vmax

  exch_1  Vmax_1

  exch_2  Vmax_2

  exch_3  Vmax_3

  .

  .

  .

//

## 12. Hill Coefficients

Deprecated. This block may be included to prevent breaking legacy layouts, but is no longer used.

References the indices in the exchange reaction block. That is, if there are 10 reaction indices, then there should be up to 10 lines in the block.

HILL_COEFFICIENTS  default_Hill

  exch_1  Hill_1

  exch_2  Hill_2

  exch_3  Hill_3

  .

  .

  .

//

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

OBJECTIVE_STYLE

objective_style_string

//

## 14. Optimizer

Only version 2.0.0 and up. Sets the optimizer. The optimizer_string can be one of the values:

- GUROBI
- GLPK

The default is GUROBI.

OPTIMIZER optimizer_string

# COMETS Parameter Descriptions

## General Comets Parameters

The global_params.txt file contains a list of parameters that are involved in the general COMETS application. Alternatively, parameters can be included within a layout file in an optional “parameters” block.

Note that all ‘boolean’ type parameters only allow “true” or “false” as their value (they do not accept 0 or 1).

| Parameter Name           | Type                    | Default                             | Description                                                  | Units |
| ------------------------ | ----------------------- | ----------------------------------- | ------------------------------------------------------------ | ----- |
| timeStep                 | decimal > 0             | 1                                   | time for each simulation step in hours                       | hours |
| deathRate                | decimal between 0 and 1 | 0.1                                 | Fraction of biomass that degrades each hour                  |       |
| maxSpaceBiomass          | decimal > 0             | 10                                  | maximum allowed biomass in a space in grams                  | grams |
| minSpaceBiomass          | decimal > 0             | 1.00E-10                            | minimum allowed biomass in a space in grams (setting to a small value > 0 might avoid some numerical errors) | grams |
| spaceWidth               | decimal > 0             | 0.1                                 | width of each space in cm                                    | cm    |
| spaceVolume (deprecated) | decimal > 0             | 1                                   | volume of each space in mL. This input is ignored in the latest versions of COMETS, where volume is the cube of the spaceWidth | mL    |
| allowCellOverlap         | boolean                 | FALSE                               | if true, allows different species to occupy the same space   |       |
| colorRelative            | boolean                 | TRUE                                | if true, colors each space relative to the space with the highest value. |       |
| showCycleTime            | boolean                 | TRUE                                | if true, shows the time it took to finish the fba cycle in the output |       |
| showCycleCount           | boolean                 | TRUE                                | if true, shows the current cycle number in the output        |       |
| pauseOnStep              | boolean                 | true (false if running as a script) | if true, pauses the simulation after completing a step       |       |
| displayLayer             | int 0..num media        | 0                                   | sets the current medium component (or biomass) to be displayed |       |
| pixelScale               | int > 0                 | 4                                   | the number of pixels to render for each space                |       |
| gridRows                 | int > 0                 | 100                                 | the number of rows in the simulation                         |       |
| gridCols                 | int > 0                 | 100                                 | the number of columns in the simulation                      |       |
| maxCycles                | int > 0 or -1           | -1 (infinite)                       | the maximum number of simulation steps to run. If set to -1, this is unlimited. |       |
| simulateActivation       | boolean                 | FALSE                               | if true, the models are activated with the set activationRate |       |
| activateRate             | decimal > 0             | 0.001                               | the value of activation rate                                 |       |
| toroidalWorld            | boolean                 | FALSE                               | If true, edge cells are considered adjacent to the cell on the opposite edge |       |
| randomSeed               | long                    | 0                                   | Seed value for the semirandom number generator               |       |

### Slideshow specific parameters

| Parameter Name         | Type             | Default | Description                                                  |
| ---------------------- | ---------------- | ------- | ------------------------------------------------------------ |
| saveSlideshow          | boolean          | FALSE   | if true, saves a graphics slideshow to a series of files. See also slideshowName. |
| slideshowColorRelative | boolean          | TRUE    | as colorRelative above, applied to the slideshow             |
| slideshowRate          | int > 0          | 1       | the number of steps between taking a slideshow picture       |
| slideshowLayer         | int 0..num media | 0       | the layer to take a snapshot of for each slideshow step, in order of media from 0 to N-1 (for N media components). If this is set to N, a snapshot of the biomass is taken. |
| slideshowExt           | String           | png     | the file extension for slideshow pictures. Currently, only “png” “bmp” and “jpg” are supported, and “png” is probably the best. |

#### Constructing GIFs and Movies

Slideshows can be converted from a bunch of image files (such as pngs) to a gif or movie file using the convert command. e.g.

convert *.png slideshow.gif

find more details on the convert command here: <http://www.astro.umd.edu/~dcr/Courses/ASTR615/intro_viz/node7.html> and here: <http://www.ma.utexas.edu/cgi-bin/man-cgi?convert+1> This command requires the program “ImageMagick” (<http://www.imagemagick.org/script/index.php>) which is installed by default on most linux distributions.

## FBA Modeling Parameters

The package_params.txt file contains a list of parameters that are particular to the FBA functions of COMETS. If other packages are created, they will probably use different sets of parameters.

### FBA parameters

| Parameter Name     | Type                                                         | Default                        | Description                                                  | Units        |
| ------------------ | ------------------------------------------------------------ | ------------------------------ | ------------------------------------------------------------ | ------------ |
| exchangeStyle      | String:{Standard FBA \| Monod Style \| Pseudo-Monod Style}   | Standard FBA                   | Sets the method used for calculating maximum exchange rates for all extracellular metabolites. Only one of the three strings is an allowed value |              |
| biomassMotionStyle | String:{“Diffusion 2D(Crank-Nicolson)”, “Diffusion 2D(Eight Point)”, “Diffusion 3D”, “Convection 2D”, “Convection 3D” } | “Diffusion 2D(Crank-Nicolson)” | Sets the method used for diffusing biomass. Only one of the indicated strings is an allowed value |              |
| numDiffPerStep     | int > 0                                                      | 10                             | Number of partial diffusions to do for each time step. Instead of doing one big jump for a time step, this gives the option of breaking the time step into smaller pieces to do a smoother diffusion calculation. |              |
| growthDiffRate     | decimal > 0                                                  | 1.00E-07                       | The default biomass diffusion constant for growth in cm^2/s  | cm2/s        |
| flowDiffRate       | decimal > 0                                                  | 1.00E-07                       | The default biomass diffusion constant for non-growth diffusion in cm^2/s | cm2/s        |
| defaultVmax        | decimal > 0                                                  | 10                             | The default Vmax value for Monod-style exchange              | mmol/gCDW/hr |
| defaultKm          | decimal > 0                                                  | 5                              | The default Km value for Monod-style exchange                | mmol/cm3     |
| defaultHill        | decimal > 0                                                  | 2                              | The default Hill coefficient for Monod-style exchange        |              |
| defaultAlpha       | decimal > 0                                                  | 1                              | The default Alpha coefficient (slope) for the Pseudo-Monod style exchange |              |
| defaultW           | decimal > 0                                                  | 10                             | The default W coefficient (plateau) for the Pseudo-Monod style exchange |              |
| defaultDiffConst   | decimal > 0                                                  | 1.00E-05                       | The default diffusion constant for extracellular metabolites |              |
| randomOrder        | boolean                                                      | TRUE                           | Should the order that models are processed at each timestep be randomized? If false, they are always run in the order presented in the first line of the layout file. |              |
| numRunThreads      | Int > 0                                                      | 1                              | Allow multithreaded computation                              |              |
| numExRxnSubsteps   | Int > 0                                                      | 5                              | Number times a timestep can be cut in half so an extracellular reaction can be calculated with increased accuracy when reactant concentrations approach 0 |              |

### Log file parameters

| Parameter Name       | Type    | Default               | Description                                                  |
| -------------------- | ------- | --------------------- | ------------------------------------------------------------ |
| useLogNameTimeStamp  | boolean | TRUE                  | If true, appends a time stamp to every log file name         |
| writeFluxLog         | boolean | FALSE                 | If true, writes fluxes out to a log file                     |
| fluxLogName          | String  | “flux_log.txt”        | The name of the flux log file                                |
| fluxLogRate          | int > 0 | 1                     | How often (number of simulation steps) to write to the flux file. A value of 1 will cause writing after every step. |
| writeMediaLog        | boolean | FALSE                 | if true, writes media information to a log file              |
| mediaLogName         | String  | media_log.txt         | The name of the media log file                               |
| mediaLogRate         | int > 0 | 1                     | How often to write to the media file                         |
| writeBiomassLog      | boolean | FALSE                 | if true, writes biomass information to a log file            |
| biomassLogName       | String  | biomass_log.txt       | The name of the biomass log file                             |
| biomassLogRate       | int > 0 | 1                     | How often to write to the biomass file                       |
| writeTotalBiomassLog | boolean | FALSE                 | if true, writes a summation of all biomass information to a log file |
| totalBiomassLogName  | String  | total_biomass_log.txt | The name of the total biomass log file                       |
| totalBiomassLogRate  | int > 0 | 1                     | How often to write to the total biomass log file             |