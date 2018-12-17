## In MATLAB-COMETS Toolbox

First, you must instantiate a layout as a member of the [CometsLayout](https://github.com/segrelab/comets-toolbox/blob/master/core/CometsLayout.m)  class.

**createLayout()**: creates an empty CometsLayout object using default parameters

**createLayout(CometsParams)**: creates an empty CometsLayout object which uses the given set of parameters

**createLayout(cobraModel)** or **createLayout([cobraModels])**: creates a CometsLayout containing the given model or models.



Once your layout is completed and modified to your liking, you can generate the layout file and the related files (namely the model files and script file) by using the [writeCometsLayout](https://github.com/segrelab/comets-toolbox/blob/master/io/writeCometsLayout.m)(CometsLayout,directory) function.

**Example Code:**

```matlab
layout = createCometsLayout();
layout = addModel(layout,cobramodel);
writeCometsLayout(layout,pwd);
```

