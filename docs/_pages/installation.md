---
permalink: /install/
title: "Installation"
---
<span>**<span style="color: red">\[update needed\]</span>**</span>

In principle, you just have to extract the archive you have
downloaded to an arbitrary directory where you have write permissions
to install SIRIUS. The Java (JRE) needed to run SIRIUS is already included.

For Windows/MacOS we also provide installer packages (msi/pkg) which should be preferred 
but might require admin permissions. Since we do not pay Microsoft/Apple for certification 
you might have to confirm that you want to trust software from an unknown source on Windows/MacOS.
On MacOS the option to confirm the execution of the installer (pkg) might be hidden under 
`"System Settings" -> "Security & Privacy"`.

If you have trouble installing SIRIUS, please let us 
[know](mailto:sirius@uni-jena.de) and we will see if we can help.
If you find that our installation guide is incomplete, or if you have
some tricks that you want to share with your fellow scientists, please
let us know so we can include them in this manual, or even better, contribute them yourself.

**Warning** All advice given here on how to get SIRIUS running on your
system, is given without any warranty! If you are not sure what you are
doing, you might want to contact someone who does. (Remember, last
Friday in July is System Administrator Appreciation Day!)

## Windows

##### Zip version
Extract the archive to an arbitrary directory where you have write
permissions, such as `C:\SIRIUS`.

##### MSI (Installer) version
Execute the installer, trust the unknown source and follow the instructions.
You will have the option to choose an installation location and need to 
accept the SIRIUS license agreement.

##### Execution 
Go to the SIRIUS directory. Run `sirius-gui.exe` to start the graphical user interface.
You might want to create a link on your desktop: Click and drag the file
to the desktop, keeping the ALT key pressed. You can rename the link on
your desktop as you like. You start SIRIUS by double-clicking this link.

If you have used the Installer you should also have a start menu entry for SIRIUS. 

Run `sirius.exe` for the SIRIUS command line tool. To execute the SIRIUS command line
tool from every location on your system, you have to add the location of
the to your PATH environment variable: Open the Windows Setting, type
"advanced" in the search window, say "yes" if Windows asks you. Press
the "Environment Variables" button, select the "Path" variable in the
lower panel, press "Edit", press "New", enter the full directory path
of SIRIUS, press RETURN. Close the Command Prompt, open a new one, type
`sirius`.

## Linux and Mac OS X

For Mac OSX, make sure that you have installed JAVA version 8 or higher
from <https://java.com/>; Apple only provides the deprecated version 6.

Extract SIRIUS into a folder of your choice, for example .

To start the SIRIUS command line tool you then have to enter:

<span> </span>

/sirius/bin/sirius

To start the graphical user interface of SIRIUS:

<span> </span>

/sirius/bin/sirius-gui

To execute SIRIUS from every location you have to add to your PATH
variable. To do so, open in an editor and add the following line
(replacing the placeholder path):

export PATH=PATH:/sirius/bin/

Note that you have to reopen your "bash" shell to make the changes
effective.

## Proxy servers

To use database related functionality of SIRIUS, it needs an Internet
connection. You have to ensure that SIRIUS is not blocked by any
security software on your computer.

If you have to use a proxy server to connect to the Internet, SIRIUS
automatically uses the system wide Java proxy configuration if
available. Alternatively you can specify the proxy configuration in the
Sirius user interface setting (see Section [6.6](#sec:settings)).

If Sirius cannot connect to the Internet, it will report on which stage
the error occurred.

## Installing Gurobi and CPLEX

SIRIUS ships with the GLPK (GNU Linear Programming Kit) Integer Linear
Program solver which allows us to swiftly compute fragmentation trees in
most cases. However, if you want to analyze large molecules and/or
spectra with many peaks and/or a large number of spectra, you can
greatly improve running time by using a faster solver. SIRIUS also
supports Integer Linear Program solvers Gurobi and CPLEX . These are
commercial solvers which offer a free academic license for university
members. You can find installation instruction on their websites. Using
Gurobi or CPLEX will greatly improve the speed of fragmentation tree
computations, which is the most time-intense step of the computational
analysis. Beside this, there will be no differences in using Gurobi,
CPLEX or GLPK. To use Gurobi set the environment variable `GUROBI_HOME`
to a valid Gurobi installation location e.g. . Similarly, to use CPLEX
set `CPLEX_HOME` to . SIRIUS will automatically use Gurobi or CPLEX as
its solver if corresponding environment variables are specified. You can
manually set the preferred ILP solvers in the settings dialog (GUI).

