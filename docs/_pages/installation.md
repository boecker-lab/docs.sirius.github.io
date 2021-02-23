---
permalink: /install/
title: "Installation"
---

In principle, installing SIRIUS just means extracting the archive you have
downloaded to an arbitrary directory where you have write permissions. 
The *Java Runtime Environment* (JRE) needed to run SIRIUS is already included.

**For Windows/MacOS** we also provide **installer packages** (msi/pkg) which **should be preferred** 
but might require admin permissions. Since we do not pay Microsoft/Apple for certification 
you might have to confirm that you want to trust software from an unknown source on Windows/MacOS.

If you have trouble installing SIRIUS, please [let us 
know](mailto:sirius@uni-jena.de) and we will see if we can help.
If you find that our installation guide is incomplete, or if you have
some tricks that you want to share with your fellow scientists, please
let us know, so we can include them in this manual, or even better, 
[contribute them yourself](https://github.com/boecker-lab/docs.sirius.github.io#contributing-to-the-sirius-documentation).

**Warning** All advice given here on how to get SIRIUS running on your
system, is given without any warranty! If you are not sure what you are
doing, you might want to contact someone who does. (Remember, last
Friday in July is System Administrator Appreciation Day!)

## Windows
##### MSI installer (preferred)
Execute the installer, trust the unknown source and follow the instructions.
You will have the option to choose an installation location and need to 
accept the SIRIUS license agreement. The installer should also create a start menu entry for SIRIUS.

##### Zip package
Extract the archive to an arbitrary directory where you have write
permissions, such as `C:\SIRIUS`.

##### Execution 
Go to the SIRIUS directory. Run `sirius-gui.exe` to start the graphical user interface.
You might want to create a link on your desktop: Click and drag the file
to the desktop, keeping the ALT key pressed. You can rename the link on
your desktop as you like. You start SIRIUS by double-clicking this link.
 

Run `sirius.exe` for the SIRIUS command line tool. To execute the SIRIUS command line
tool from every location on your system, you have to add the location of
the to your PATH environment variable: Open the Windows Setting, type
"advanced" in the search window, say "yes" if Windows asks you. Press
the "Environment Variables" button, select the "Path" variable in the
lower panel, press "Edit", press "New", enter the full directory path
of SIRIUS, press RETURN. Close the Command Prompt, open a new one, type
`sirius`.

## Mac OSX
##### pkg installer (preferred)
Execute the installer, trust the unknown source and follow the instructions.
You will have the option to choose an installation disk and need to 
accept the SIRIUS license agreement.
The option to confirm the execution an installer form an unknown might be "hidden" under 
`"System Settings" -> "Security & Privacy"`.

##### Zip package
Extract the archive to an arbitrary directory where you have write
permissions, e.g. Download folder. You can then move/copy the extracted `.app` 
folder to your application directory. You might have to define gate keeper exceptions
for all libraries in the SIRIUS `.app` directory. If you are not sure how to do this
we recommend using the installer version.  

##### Execution 
To run the SIRIUS GUI just go to you app directory an double click the `sirius-gui` app.
You can also add SIRIUS to your dock if you like.  

To start the SIRIUS command line tool open a terminal and execute execute 
the `sirius` launcher in your Application directory (usually `/Applications/sirius.app/Contents/MacOS/sirius`).

To execute SIRIUS from every location you have to add the sirius binary directory `<SIRIUS_DIR>/Contents/MacOS` to your PATH
variable. To do so, open `/etc/paths` in a text editor and add the following line:

```
/Applications/sirius.app/Contents/MacOS/
```

In case you have changed it, replace the line with the appropriate sirius installation dir.
Note that the SIRIUS GUI versio also contains the command line runner, but uses a slieghly different location er default:
```
/Applications/sirius.app/Contents/MacOS/
```

## Linux
##### Zip version
Extract the archive to an arbitrary directory where you have write
permissions, e.g. `/home/opt/sirius`.

To start the commandline version of SIRIUS execute the 
`<SIRIUS_DIR>/bin/sirius` starter in the terminal.

To start the graphical user interface of SIRIUS execute the 
`<SIRIUS_DIR>/bin/sirius-gui` starter in the terminal.

To execute SIRIUS from every location you have to add the `<SIRIUS_DIR>/bin` to your PATH
variable. To do so, open in an editor and add the following line
(replacing the placeholder path) to your `~/.bashrc`:

```
export PATH=PATH:<SIRIUS_DIR>/bin/
```

Note that you have to reopen your "bash" shell to make the changes
effective.

## Installing Gurobi and/or CPLEX

SIRIUS ships with the *COIN-OR* Integer Linear
Program solver which allows us to swiftly compute fragmentation trees in
most cases. However, if you want to analyze large molecules and/or
spectra with many peaks and/or many spectra, you can
improve running time by using a faster solver. SIRIUS also
supports Integer Linear Program solvers *Gurobi* and *CPLEX*. These are
commercial solvers which offer a free academic license for university
members. You can find installation instruction on their websites. Using
*Gurobi* or *CPLEX* will improve the speed of fragmentation tree
computations, which is the most time-intense step of the computational
analysis. Besides, there will be no differences in using *Gurobi*,
*CPLEX*. To use Gurobi set the environment variable `GUROBI_HOME`
to a valid Gurobi installation location. 
Similarly, to use *CPLEX* set `CPLEX_HOME` to a valid Gurobi installation location.

#### CPLEX
On Windows the CPLEX installer usually sets the system variable automatically, and you should be good to go.
* Windows: set `CPLEX_HOME` to e.g. `C:\Program Files\IBM\ILOG\CPLEX_Studio1271\cplex`.
* Linux: Set `CPLEX_HOME` to the CPLEX install directory e.g. `/opt/ibm/ILOG/CPLEX_Studio1271/cplex`.
* MacOS: Set `CPLEX_HOME` to the CPLEX install directory e.g. `/Library/ibm/ILOG/CPLEX_Studio1271/cplex`.

#### Gurobi
* Windows: Set `GUROBI_HOME` to the Gurobi lib directory e.g. `C:\gurobi702\win64`.
* Linux: Set `GUROBI_HOME` to the Gurobi lib directory e.g. `/opt/gurobi702/linux64`.
* MacOS: Set `GUROBI_HOME` to the Gurobi lib directory e.g. `/Library/gurobi702/mac64`.


SIRIUS will automatically detect Gurobi or CPLEX as possible solver if corresponding environment 
variables are specified. You can specify the preferred solvers in the settings 
dialog (GUI) or when running the command line with the `--ilp-solver` parameter.
To permanently change the setting (e.g. for SIRIUS version 4.6.x) open: 
`<USER_HOME>/.sirius-4.6/sirius.properties`  and modify the following line to your needs:

```properties
de.unijena.bioinf.sirius.treebuilder.solvers = cplex,gurobi,clp
```
The order of the solvers specifies the priority in which SIRIUS uses them.


## Proxy servers

To use database related functionality of SIRIUS, it needs an Internet
connection. You have to ensure that SIRIUS is not blocked by any
security software on your computer.

If you have to use a proxy server to connect to the Internet, SIRIUS
automatically uses the system wide Java proxy configuration if
available. Alternatively you can specify the proxy configuration in the
Sirius user interface setting (see [Settings]({{ "//gui/#settings" | relative_url }})).

If SIRIUS cannot connect to the Internet, it will [report]({{ "//gui/#webservice" | relative_url }}) on which stage
the error occurred.