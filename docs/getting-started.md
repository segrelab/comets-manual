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

​	$tar -zxvf COMETS_X.Y.Z_mac.tar.gz

where X.Y.Z are replaced with the version numbers.

This will create a directory COMETS_X.Y.Z_mac/comets_X.Y.Z. It contains INSTALL file. To install COMETS read the INSTALL file.

### Installing on Linux

COMETS for Linux is distributed as an archived COMETS_X.Y.Z.tar.gz file, where X.Y.Z is the version number. To extract it, run it in a terminal:

​	$tar -zxvf COMETS_X.Y.Z.tar.gz

where X.Y.Z are replaced with the version numbers.

This will create a directory COMETS_X.Y.Z/comets_X.Y.Z. It contains INSTALL file. To install COMETS read the INSTALL file.

## Running COMETS

The instructions for running COMETS are in the RUNNING_COMETS file in the comets_X.Y.Z directory.

The comets_X.Y.Z/layouts_and_models directory contains three examples of layouts and two models. The model_CSP_petri_dish_circles is an example of a two dimensional layout with a toy model model_CSP. The model_CSP_3D_layout is a three dimensional layout with the same toy model.

The Ecoli_colony_layout is a layout for growth of an E.coli colony in two dimensions. The corresponding model is in the EC_ijo1366_model file. A detailed description of running this layout and model is [here](http://www.bu.edu/segrelab/comets-e-coli-simulation-example/).