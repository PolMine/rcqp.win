## Purpose

The package is a fork of the 'rcqp' package authored by Bernard Desgraupes
and Sylvain Loiseau. See the plain text README file in this folder for 
further information. 

The original rcqp package has been modified slightly to make the installation
on Windows as easy as possible.

## Installation

The package can be installed directly from GitHub using devtools.

```{r}
install.packages("devtools") # if not yet available
devtools::install_github("PolMine/rcqp.win")
```
Alternatively, a pre-built binary is available via the server of the PolMine
Project.

```{r}
install.packages("rcqp", repos = "http://polmine.sowi.uni-due.de/packages", type = "win.binary")
```

## Documentation

The package is derived from the 'rcqp' package through a set of (minimal)
modifications. Preparing this package requires a system that has the compiled
libraries (libintl, libglib etc.) present.

To following shell commands are executed in the directory of the rcqp.win package.

```{sh}
wget https://cran.r-project.org/src/contrib/rcqp_0.4.tar.gz
tar xzfv rcqp_0.4.tar.gz
rm rcqp_0.4.tar.gz
mv rcqp/* .
rm -r rcqp
rm -r src # the C source code is not needed
rm configure*
rm cleanup
rm MD5
```

The dependant libraries have then copied into a inst/libs/i386 and a inst/libst/x86 directory.



## Feedback

Your feedback is highly welcome! Please write to Andreas Blaette (andreas.blaette@uni-due.de)
