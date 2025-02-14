# R-package---.lif-image-analysis
R package allowing .lif image processing 
README
lif image simplification
library(Lif.image.simplification)
#> Le chargement a nécessité le package : webshot
#> Le chargement a nécessité le package : colocr
#> Le chargement a nécessité le package : imager
#> Le chargement a nécessité le package : magrittr
#> 
#> Attachement du package : 'imager'
#> L'objet suivant est masqué depuis 'package:magrittr':
#> 
#>     add
#> Les objets suivants sont masqués depuis 'package:webshot':
#> 
#>     resize, shrink
#> Les objets suivants sont masqués depuis 'package:stats':
#> 
#>     convolve, spectrum
#> L'objet suivant est masqué depuis 'package:graphics':
#> 
#>     frame
#> L'objet suivant est masqué depuis 'package:base':
#> 
#>     save.image
#> Le chargement a nécessité le package : remotes
Presentation
lif image simplification is a package that has been developed to simplify the analyse of lif images by R. It gives the possibility to choose a region of interest and to crop or return it under different way. It is made to choose the best frame of an 3D image and crop around the region of interest, so it can be used in a report or a presentation. It is not strictly image analysis this is why the name is “lif image simplification”.

Details about the package
This package use the function read.Image from the package “RBioFormats” to open and read the lif file. This function transform the image into an image object provided by the package “EBImage”, called “Image”. This kind of object are, in some certain ways, not convenient to work with because the array of the image has the dimensions (x, y, c, z), c being the colour channel and z the frames.

Moreover, to find the region of interest (ROI) we used the function roi_select() of the package “colocr” which use the image object from the package “imager”, called “cimg”. This type of image has the shape (x, y, z, c), with the dimensions z and c inverted compared to Image object from EBImage.

For this, all function of this package have hidden conversions between EBImage::Image and imager::cimg object, but all functions return a cimg object, that is for this utilisation, easier to handle.

Requirements
The package lif.image.simplification use several packages from CRAN and Biomanager. Normally, if the package install correctly it should be no problem to install dependences from CRAN. There can be some problems for the Biomanager packages so here is provided few lines that should resolve it. Also, the package RBioFormats use versions of Java to open the file, so make sure that Java JDK 1.8 or higher is installed.

If a problem persists, don’t hesitate to contact me by email, by specifying the error.

if (!require("BiocManager", quietly=TRUE)) install.packages("BiocManager")
BiocManager::install("aoles/RBioFormats", force=TRUE)
#> 'getOption("repos")' replaces Bioconductor standard repositories, see
#> 'help("repositories", package = "BiocManager")' for details.
#> Replacement repositories:
#>     CRAN: https://cran.rstudio.com/
#> Bioconductor version 3.17 (BiocManager 1.30.20), R 4.3.0 (2023-04-21 ucrt)
#> Installing github package(s) 'aoles/RBioFormats'
#> Downloading GitHub repo aoles/RBioFormats@HEAD
#> jsonlite (1.8.4 -> 1.8.5) [CRAN]
#> Installing 1 packages: jsonlite
#> le package 'jsonlite' a été décompressé et les sommes MD5 ont été vérifiées avec succés
#> 
#> Les packages binaires téléchargés sont dans
#>  C:\Users\cyril\AppData\Local\Temp\RtmpEfzKJd\downloaded_packages
#> ── R CMD build ─────────────────────────────────────────────────────────────────
#>   
  
  
   checking for file 'C:\Users\cyril\AppData\Local\Temp\RtmpEfzKJd\remotes10a8364b45f5\aoles-RBioFormats-5ec5fad/DESCRIPTION' ...
  
   checking for file 'C:\Users\cyril\AppData\Local\Temp\RtmpEfzKJd\remotes10a8364b45f5\aoles-RBioFormats-5ec5fad/DESCRIPTION' ... 
  
✔  checking for file 'C:\Users\cyril\AppData\Local\Temp\RtmpEfzKJd\remotes10a8364b45f5\aoles-RBioFormats-5ec5fad/DESCRIPTION' (949ms)
#> 
  
  
  
─  preparing 'RBioFormats':
#>    checking DESCRIPTION meta-information ...
  
   checking DESCRIPTION meta-information ... 
  
✔  checking DESCRIPTION meta-information
#> 
  
  
  
─  checking for LF line-endings in source and make files and shell scripts
#> 
  
  
  
─  checking for empty or unneeded directories
#> 
  
  
  
─  building 'RBioFormats_0.99.14.tar.gz'
#> 
  
   
#> 
#> Installation paths not writeable, unable to update packages
#>   path: C:/Program Files/R/R-4.3.0/library
#>   packages:
#>     class, KernSmooth, MASS, Matrix, nnet
#> Old packages: 'RBioFormats', 'jsonlite'
BiocManager::install("EBImage", force=TRUE)
#> 'getOption("repos")' replaces Bioconductor standard repositories, see
#> 'help("repositories", package = "BiocManager")' for details.
#> Replacement repositories:
#>     CRAN: https://cran.rstudio.com/
#> Bioconductor version 3.17 (BiocManager 1.30.20), R 4.3.0 (2023-04-21 ucrt)
#> Installing package(s) 'EBImage'
#> le package 'EBImage' a été décompressé et les sommes MD5 ont été vérifiées avec succés
#> 
#> Les packages binaires téléchargés sont dans
#>  C:\Users\cyril\AppData\Local\Temp\RtmpEfzKJd\downloaded_packages
#> Installation paths not writeable, unable to update packages
#>   path: C:/Program Files/R/R-4.3.0/library
#>   packages:
#>     class, KernSmooth, MASS, Matrix, nnet
#> Old packages: 'RBioFormats', 'jsonlite'
remotes::install_github("ornelles/EBImageExtra")
#> Skipping install of 'EBImageExtra' from a github remote, the SHA1 (fd428511) has not changed since last install.
#>   Use `force = TRUE` to force installation
Example of utilisation
To start with, we open the image using the read image lif function. The argument that must be passed are the name of the file and the serie of the image in the file (which image do we want in the file). The serie can be or a single number or a a vector of some numbers.

(image <- read.images.lif("../data/centrioles.lif", serie=10))
#> Image. Width: 256 pix Height: 256 pix Depth: 21 Colour channels: 3

(images <- read.images.lif("../data/centrioles.lif", serie=6:8))
#> [[1]]
#> Image. Width: 256 pix Height: 256 pix Depth: 23 Colour channels: 3 
#> 
#> [[2]]
#> Image. Width: 256 pix Height: 256 pix Depth: 18 Colour channels: 3 
#> 
#> [[3]]
#> Image. Width: 256 pix Height: 256 pix Depth: 18 Colour channels: 3
In the first case it returns a single cimg, in the second case it returns a list of all cimg.

To show the image, the easiest is to use the function show.image. This function takes as argument only one image. The point of showing all image in a list way not really relevant as one image contains several frame, and takes already the whole space. But it can be the subject of a future improvment. Here the second argument “methode” is specified as “raster”, to show all the frame side by side in the markdown. The default argument is “browser” that open a viewer convenient to work with in standart analysis.

show.image(image, methode="raster")


We can notice that the eight or nine first frames are aberrant.

After reading the image, we can use the function select roi to select the region of interest. This is made by choosing several parameter as the threshold of pixel intensity and the number of region of interest that we want.

Here, we chose the parameter for the function as var(). This parameter apply the chosen function on every frame and take the frame with the higher value. Usually sum() works well, but for this image var() was better. Other functions were not try but as soon as it return a value that can be compared with values of other frames it should work as well. The threshold is set to 99.5, the number of ROI to 2 and we choose to exclude the first 11 frames that were background noise.

roi <- select.roi(image, fun=var, threshold=99.5, n=2, from.frame=11)


roi
#> [[1]]
#> [[1]]$x
#> [1] 116
#> 
#> [[1]]$y
#> [1] 86
#> 
#> [[1]]$z
#> [1] 18
#> 
#> 
#> [[2]]
#> Image. Width: 256 pix Height: 256 pix Depth: 1 Colour channels: 3
This function display the best frame, the roi and the two channel colors with the roi. It also return a list with the coordinates of the roi and the best frame as a cimg. For future improvement, we will try to adapt the function to take a list of image as an input.

Once the region of interest has been identify and the coordinates are extracted by the function select roi, they can be passed in the function crop and resize.

This function takes as argument the image, a list of the coordinates of the roi and the size of the square that is drawn around the roi can also be decided.

croppedimage <- crop.and.resize(image, roi[[1]])
#> Le chargement a nécessité le package : EBImage
#> 
#> Attachement du package : 'EBImage'
#> Les objets suivants sont masqués depuis 'package:imager':
#> 
#>     channel, dilate, display, erode, resize, watershed
#> L'objet suivant est masqué depuis 'package:webshot':
#> 
#>     resize


#> Warning in as.cimg.array(data): Assuming third dimension corresponds to colour


This function return the inside of the square, which is an image centered on the region of interest. This image can be save as text file or as jpeg as follow.

save(croppedimage, file="cropped_image.txt")
save(croppedimage, file="cropped_image.png")
about the data provided in example
A quick word about the data that is provided as an example. It is a lif file, composed of 36 images of centriole (a cellular organelle). Each image has a different number of frames. Every image is taken in two colors, red and green and every size (x and y dimensions) are 256 pixels. Every image, once imported, is similar to an array of value of 4 dimensions (x, y, z, c) (for a cimg) and each value of this array correspond to the value of the pixel at coordinates x, y, z in the color c.  Through the package, the image is transformed into several object that use the RGB color system, for Red Green Blue.

session info
sessionInfo()
#> R version 4.3.0 (2023-04-21 ucrt)
#> Platform: x86_64-w64-mingw32/x64 (64-bit)
#> Running under: Windows 11 x64 (build 22621)
#> 
#> Matrix products: default
#> 
#> 
#> locale:
#> [1] LC_COLLATE=French_Switzerland.utf8  LC_CTYPE=French_Switzerland.utf8   
#> [3] LC_MONETARY=French_Switzerland.utf8 LC_NUMERIC=C                       
#> [5] LC_TIME=French_Switzerland.utf8    
#> 
#> time zone: Europe/Zurich
#> tzcode source: internal
#> 
#> attached base packages:
#> [1] stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] EBImage_4.42.0                 BiocManager_1.30.20           
#> [3] Lif.image.simplification_0.2.1 remotes_2.4.2                 
#> [5] imager_0.45.2                  magrittr_2.0.3                
#> [7] colocr_0.1.1                   webshot_0.5.4                 
#> 
#> loaded via a namespace (and not attached):
#>  [1] xfun_0.39            bslib_0.4.2          htmlwidgets_1.6.2   
#>  [4] processx_3.8.1       rJava_1.0-6          lattice_0.21-8      
#>  [7] callr_3.7.3          generics_0.1.3       vctrs_0.6.2         
#> [10] tools_4.3.0          ps_1.7.5             bitops_1.0-7        
#> [13] stats4_4.3.0         curl_5.0.0           tibble_3.2.1        
#> [16] fansi_1.0.4          highr_0.10           pkgconfig_2.0.3     
#> [19] desc_1.4.2           S4Vectors_0.38.1     lifecycle_1.0.3     
#> [22] compiler_4.3.0       stringr_1.5.0        munsell_0.5.0       
#> [25] tiff_0.1-11          httpuv_1.6.11        htmltools_0.5.5     
#> [28] sass_0.4.6           RCurl_1.98-1.12      yaml_2.3.7          
#> [31] pillar_1.9.0         crayon_1.5.2         later_1.3.1         
#> [34] jquerylib_0.1.4      ellipsis_0.3.2       cachem_1.0.8        
#> [37] RBioFormats_1.0.0    magick_2.7.4         abind_1.4-5         
#> [40] mime_0.12            tidyselect_1.2.0     bmp_0.3             
#> [43] locfit_1.5-9.7       digest_0.6.31        stringi_1.7.12      
#> [46] dplyr_1.1.2          purrr_1.0.1          rprojroot_2.0.3     
#> [49] fastmap_1.1.1        grid_4.3.0           EBImageExtra_0.0.0.2
#> [52] colorspace_2.1-0     cli_3.6.1            utf8_1.2.3          
#> [55] pkgbuild_1.4.0       withr_2.5.0          prettyunits_1.1.1   
#> [58] scales_1.2.1         promises_1.2.0.1     rmarkdown_2.22      
#> [61] fftwtools_0.9-11     jpeg_0.1-10          igraph_1.4.3        
#> [64] png_0.1-8            shiny_1.7.4          evaluate_0.21       
#> [67] knitr_1.43           rlang_1.1.1          readbitmap_0.1.5    
#> [70] Rcpp_1.0.10          xtable_1.8-4         glue_1.6.2          
#> [73] BiocGenerics_0.46.0  rstudioapi_0.14      jsonlite_1.8.4      
#> [76] R6_2.5.1
