#---------------------------------
#------ R tips and tricks--------
#--------------------------------
# to install packages to directory
# the is a file to tell which directory R will  install the packages called .Renviron
# redirect to main directory and check it
cd 
touch .Renviron
# modify the file
vim .Renviron
# put the directory you want to 
R_LIBS="/shares/common/users/m.tran/R_packages"
# save and close
install.packages("RPackage", lib="/90days/s4596423/st_env")
#load the package installed locally
library("myRPackage", lib.loc="/usr/me/local/R/library")


#----------------------------------------
#--------Install Rdata.table in MacOS----
#----------------------------------------
install.packages("data.table", type = "binary")

#------------------------------------------------------------------------
#---------Install Seurat for single cell analysis on delta cluster-------
#------------------------------------------------------------------------
Step 1: load R module `module load R/3.6.2` and open Rstudio: `R`
Step 2: install R devetools: `install.packages('devtools', loc='/shares/common/users/m.tran/R_packages')`
Step 3: Run install Seurat
install.packages('Seurat', lib='/shares/common/users/m.tran/R_packages')
Output error may have is
Warning messages:
1: In install.packages("Seurat", lib = "/shares/common/users/m.tran/R_packages") :
  installation of package 'RcppParallel' had non-zero exit status
2: In install.packages("Seurat", lib = "/shares/common/users/m.tran/R_packages") :
  installation of package 'uwot' had non-zero exit status
3: In install.packages("Seurat", lib = "/shares/common/users/m.tran/R_packages") :
  installation of package 'mutoss' had non-zero exit status
4: In install.packages("Seurat", lib = "/shares/common/users/m.tran/R_packages") :
  installation of package 'metap' had non-zero exit status
5: In install.packages("Seurat", lib = "/shares/common/users/m.tran/R_packages") :
  installation of package 'Seurat' had non-zero exit status
Solved 1: 
1.resolve the first one by running the commands: 'module unload gcc/5.2.0', `module load gcc/7.2.0` 
2. Download RcppParallel package from CRAN website/link below
https://cran.r-project.org/src/contrib/RcppParallel_4.4.4.tar.gz
then go back to R and run the command:
install.packages("/shares/common/users/m.tran/RcppParallel_4.4.4.tar.gz", lib='/shares/common/users/m.tran/R_packages/', repos=NULL, type='source')
Solved 2: 
1. After installing RcppParallel, run the command:
install.packages("uwot", lib='/shares/common/users/m.tran/R_packages/')
Solved 3+4: 
Run the R install BiocManger
1. install.packages("BiocManager")
2. Install it through Biocmanager can help
To tell R to use both CRAN and Bioconductor repos when installing packages, run the following command in R
setRepositories(ind = 1:2)
Solved 5: 
Off course the last step is to rerun the command install.packages('Seurat', lib='/shares/common/users/m.tran/R_packages')
--> Problem solved

#------------------------------------------------------
#----------Run Seurat and Error log -------------------
#------------------------------------------------------
# when I tried to run UMap dimensonal reduction with

seurat_obj <- RunUMAP(seurat_obj, dims = 1:10)

got the error
```
Error in initialize_python(required_module, use_environment) : 
Python shared library not found, Python bindings not loaded.
```

Resolve it by running
library(reticulate)
py_install("umap-learn")
-->> doesn't work well
Solution: 

# point to python app the run the umap
after importing the `reticulate`
In R studio run the following
use_python("/90days/s4596423/envs/r_seurat/bin/python")

if should work from now
Warning: The default method for RunUMAP has changed from calling Python UMAP via reticulate to the R-native UWOT using the cosine metric

###########################################################################
seurat_obj <- RunUMAP(seurat_obj, dims = 1:10)

Traceback:
 1: RcppParallel::setThreadOptions(numThreads = n_threads)
 2: uwot

This error can occur if you install seurat in the conda environment yet install RcppParallel from package or CRan. 
Solution run the below command, it will fix the crashing of R:
`conda install r-rcppparallel -c conda-forge`

Go back to the directoy where you installed RcppParallel package and remove it. 
Rerun the script and it should work from now
###########################################################################
VlnPlot(df, features = c("nFeature_RNA", "nCount_RNA", "percent.mt"), ncol = 3)

Error
OMP: Error #34: System unable to allocate necessary resources for OMP thread:
OMP: System error #11: Resource temporarily unavailable
OMP: Hint Try decreasing the value of OMP_NUM_THREADS.
Aborted

Solution: export OMP_NUM_THREADS=1

############################################################################
seurat_obj.markers %>% group_by(cluster) %>% top_n(n = 2, wt = avg_logFC)

Error:
Error in seurat_obj.markers %>% group_by(cluster) %>% top_n(n = 2, wt = avg_logFC) : 
  could not find function "%>%"
Solution:

###########################################################################
Possible error after installing seurat in conda environ

Launch jupyter notebook requires R r-essential and all related r packages to be installed in the environment
A possible error relates to /stringi.......

Solution 
In the terminal script, run the following command

`conda install -c r r-stringi`

Activate Rstudio

install.packages(stringi)
############################################################################
Another possible error is: 
Error: package or namespace load failed for ‘Seurat’ in loadNamespace(i, c(lib.loc, .libPaths()), versionCheck = vI[[i]]):
 namespace ‘rlang’ 0.3.4 is already loaded, but >= 0.4.0 is required

Solution:
In the terminal run
conda install -c conda-forge r-rlang



