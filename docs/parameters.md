# COMETS Parameter Descriptions

## General Comets Parameters

The global_params.txt file contains a list of parameters that are involved in the general COMETS application. Alternatively, parameters can be included within a layout file in an optional “parameters” block.

Note that all ‘boolean’ type parameters only allow “true” or “false” as their value (they do not accept 0 or 1).

| Parameter Name           | Type                    | Default                             | Description                                                                                                                    | Units |
| ------------------------ | ----------------------- | ----------------------------------- | ------------------------------------------------------------                                                                   | ----- |
| timeStep                 | decimal > 0             | 1                                   | time for each simulation step in hours                                                                                         | hours |
| deathRate                | decimal between 0 and 1 | 0.1                                 | Fraction of biomass that degrades each hour                                                                                    |       |
| maxSpaceBiomass          | decimal > 0             | 10                                  | maximum allowed biomass in a space in grams                                                                                    | grams |
| minSpaceBiomass          | decimal > 0             | 1.00E-10                            | minimum allowed biomass in a space in grams (setting to a small value > 0 might avoid some numerical errors)                   | grams |
| spaceWidth               | decimal > 0             | 0.1                                 | width of each space in cm                                                                                                      | cm    |
| spaceVolume (deprecated) | decimal > 0             | 1                                   | volume of each space in mL. This input is ignored in the latest versions of COMETS, where volume is the cube of the spaceWidth | mL    |
| allowCellOverlap         | boolean                 | FALSE                               | if true, allows different species to occupy the same space                                                                     |       |
| colorRelative            | boolean                 | TRUE                                | if true, colors each space relative to the space with the highest value.                                                       |       |
| showCycleTime            | boolean                 | TRUE                                | if true, shows the time it took to finish the fba cycle in the output                                                          |       |
| showCycleCount           | boolean                 | TRUE                                | if true, shows the current cycle number in the output                                                                          |       |
| pauseOnStep              | boolean                 | true (false if running as a script) | if true, pauses the simulation after completing a step                                                                         |       |
| displayLayer             | int 0..num media        | 0                                   | sets the current medium component (or biomass) to be displayed                                                                 |       |
| pixelScale               | int > 0                 | 4                                   | the number of pixels to render for each space                                                                                  |       |
| gridRows                 | int > 0                 | 100                                 | the number of rows in the simulation                                                                                           |       |
| gridCols                 | int > 0                 | 100                                 | the number of columns in the simulation                                                                                        |       |
| maxCycles                | int > 0 or -1           | -1 (infinite)                       | the maximum number of simulation steps to run. If set to -1, this is unlimited.                                                |       |
| simulateActivation       | boolean                 | FALSE                               | if true, the models are activated with the set activationRate                                                                  |       |
| activateRate             | decimal > 0             | 0.001                               | the value of activation rate                                                                                                   |       |
| toroidalWorld            | boolean                 | FALSE                               | If true, edge cells are considered adjacent to the cell on the opposite edge                                                   |       |
| randomSeed               | long                    | 0                                   | Seed value for the semirandom number generator                                                                                 |       |


### Slideshow specific parameters

| Parameter Name         | Type             | Default | Description                                                                                                                                                                 |
| ---------------------- | ---------------- | ------- | ------------------------------------------------------------                                                                                                                |
| saveSlideshow          | boolean          | FALSE   | if true, saves a graphics slideshow to a series of files. See also slideshowName.                                                                                           |
| slideshowColorRelative | boolean          | TRUE    | as colorRelative above, applied to the slideshow                                                                                                                            |
| slideshowRate          | int > 0          | 1       | the number of steps between taking a slideshow picture                                                                                                                      |
| slideshowLayer         | int 0..num media | 0       | the layer to take a snapshot of for each slideshow step, in order of media from 0 to N-1 (for N media components). If this is set to N, a snapshot of the biomass is taken. |
| slideshowExt           | String           | png     | the file extension for slideshow pictures. Currently, only “png” “bmp” and “jpg” are supported, and “png” is probably the best.                                             |

#### Constructing GIFs and Movies

Slideshows can be converted from a bunch of image files (such as pngs) to a gif or movie file using the convert command. e.g.

```
convert *.png slideshow.gif
```

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
