# Installation


To install SIRIUS, you only have to extract the archive you have
downloaded to an arbitrary directory where you have write permissions.
To run SIRIUS, you need to have a **Java Runtime Environment (JRE)
version 11 or higher** installed. **If you run a 64-bit operating
system, you also have to install a 64-bit Java runtime environment\!**
If you have trouble installing SIRIUS, please let us know at
[sirius@uni-jena.de](sirius@uni-jena.de) and we will see if we can help.
If you find that our installation guide is incomplete, or if you have
some tricks that you want to share with your fellow scientists, please
let us know so we can include them in this manual.

**Warning\!** All advice given here on how to get SIRIUS running on your
system, is given without any warranty\! If you are not sure what you are
doing, you might want to contact someone who does. (Remember, last
Friday in July is System Administrator Appreciation Day\!)

## Windows

Extract the archive to an arbitrary directory where you have write
permissions, such as `C:\SIRIUS`.

To check if you have a 64-bit Windows operating system, open the Windows
settings, go to “System”, then “Info”. You will see a line “System type”
that tells you if your operating system is 32- or 64-bit. To check if
your Java version is 64-bit, open a Command Prompt (press the Windows
key, then start typing “command prompt”, start the Command Prompt) and
type . If Windows complains that the java command is unknown, type `cd
C:\ProgramData\Oracle\Java\javapath`, then . In the last line of the
output, you will see if you have 64-bit Java installed. If this is not
the case, but your Windows is 64-bit, you have to install the correct
Java version from <https://java.com/>. Press “Download”, but *do not use
the big red button to download Java\!* Instead, go to “See all Java
Downloads”, then choose “Windows 64-bit”.

Go to the SIRIUS directory. Run to start the graphical user interface.
You might want to create a link on your desktop: Click and drag the file
to the desktop, keeping the ALT key pressed. You can rename the link on
your desktop as you like. You start SIRIUS by double-clicking this link.

Run for the SIRIUS command line tool. To execute the SIRIUS command line
tool from every location on your system, you have to add the location of
the to your PATH environment variable: Open the Windows Setting, type
“advanced” in the search window, say “yes” if Windows asks you. Press
the “Environment Variables” button, select the “Path” variable in the
lower panel, press “Edit…”, press “New”, enter the full directory path
of SIRIUS, press RETURN. Close the Command Prompt, open a new one, type
.

## Linux and Mac OS X

For Mac OS X, make sure that you have installed JAVA version 8 or higher
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

xport PATH=PATH:\~/sirius/bin/

Note that you have to reopen your ”bash” shell to make the changes
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
CPLEX or GLPK. To use Gurobi set the environment variable GUROBI\_HOME
to a valid Gurobi installation location e.g. . Similarly, to use CPLEX
set CPLEX\_HOME to . SIRIUS will automatically use Gurobi or CPLEX as
its solver if corresponding environment variables are specified. You can
manually set the preferred ILP solvers in the settings dialog (GUI).

