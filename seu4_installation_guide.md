
Instalation of seurat 4 on DKFZ rstudio worker. To install seurat 4 on normal ubuntu steps 0 and 4 should be enough.
R version = 4.4.0

# 0. Remove Previous Versions of Seurat from All Libraries

Get all library paths:

```r
lib_paths <- .libPaths()
```

Loop through each library path and uninstall Seurat:

```r
for (lib in lib_paths) {
  try(remove.packages("Seurat", lib = lib), silent = TRUE)
}
for (lib in lib_paths) {
  try(remove.packages("SeuratObject", lib = lib), silent = TRUE)
}
```

Check if you have really uninstalled Seurat:

```r
library(Seurat) # This should return an error now
```

# 1. Set libPaths to a Folder You Own

For example (in R console):

```r
.libPaths(c("/home/l675r/R/x86_64-pc-linux-gnu-library/4.4"))

# or

tmp <- .libPaths()
tmp <- c("/omics/odcf/analysis/OE0014_projects/tilbrm/results_kJacek/4.4", tmp)
.libPaths(tmp)
```

# 2. Add the Following Lines to the End of ~/.bash_profile

In bash console:

```bash
nano ~/.bash_profile
```

Then add:

```bash
if ps -e | grep -q rserver; then
  module load gdal/3.0.2
  module load hdf5/1.8.18
  module load gcc/12.2.0
  module load binutils/2.34
  module load libpng/1.6.37
  module load jags/4.3.0
  module load freetype/2.10.0
  module load imagemagick/6.9.12
fi
```

# 3. Change C++ Version (uwot Package Needs CXX14 and sctransform CXX17)

Create `Makevars` files in `~/.R` using bash console:

```bash
mkdir ~/.R
cd ~/.R
nano Makevars
```

Add these lines to the `Makevars` file (which is empty):

```bash
CXX_STD = CXX17

CC=gcc
CXX=g++
C11=g++
C14=g++
FC=gfortran
F77=gfortran

CXX14 = g++
CXX14FLAGS = -std=c++14
CXX14STD = -std=c++14

CXX17 = g++
CXX17FLAGS = -std=c++17 -fPIC
CXX17STD = -std=c++17
```

Remember not to leave any empty lines at the end of the `.bash_profile` and `Makevars` files.

# 4. Install Seurat 4

Quit your session (DO NOT SAVE IT).

Start a new R session to apply all the changes.

Then, in the R console:

```r
options(repos = c("https://satijalab.r-universe.dev", getOption("repos")))
remotes::install_version("SeuratObject", "4.1.4")
remotes::install_version("Seurat", "4.4.0", upgrade = FALSE)
```
