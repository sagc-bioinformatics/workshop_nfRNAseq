+++
title = 'nf-core/DifferentialAbundance'
+++

![DifferentialAbundance_pipeline](DifferentialAbundance_pipeline.png)

#### nf-core/DifferentialAbundance

There are a few ways of installing the nf-core pipeline. But this happens automatically when you use a *nexflow run nf-core/* commands.
You can check the ~/.nextflow/assets folders to see what is already installed

```
ls -l ~/.nextflow/assets/nf-core/
```
If you don't see the pipeline, you can pull it from the nf-core website. 
```bash
nextflow pull nf-core/differentialabundance
```
but this happeds automatically when running the nextflow run nf-core/differentialabundance

![nfpull](nextflowpull.png)



# R Shiny App
- one of the key outputs from the nf-core/DifferentialAbundance pipeline is an Shiny app.
- This runs R processes in the background and presents the data as a site.
- One way to run the app is to kick it off in R studio, to do this you can download the App directory and use the following R code to open it.

### download the app directory to you local pc and use R studio
***optional*** 

The easiest way to run your R shiny app is using R studio, using the following code, howover on the day we are going to host the App with the same Nectar VM you have been using.

Firstly download the nf-core/DifferentialAbundance outputs which you have generated to your pc using scp

```bash
IP=[yourIPaddress]

mkdir -p  ~/workshop_RNAseq/nfDifferentialAbundance 
scp -r workshop@$IP:/home/workshop/workshop/nfDifferentialAbundance/outs/ ~/workshop_RNAseq/nfDifferentialAbundance/

cd ~/workshop_RNAseq/nfDifferentialAbundance/outs
```

Then open Rstudio and copy this code 
```r
##To run shiny App Locally
# run this in R studio

#######################

# install packages
install.packages("remotes")
remotes::install_github("pinin4fjords/shinyngs")
library(remotes)
library(shinyngs)
library(markdown)

#######################

# navigate to where the shiny app is, where you have downloaded it to.

setwd("/Users/danielthomson/workshop_RNAseq/nfDifferentialAbundance/outs/shinyngs_app/SAGC_Workshop_RNAseq")

#####
esel <- readRDS("data.rds") # your need to navigate to the 'shinyngs_app' directory
app <- prepareApp("rnaseq", esel)
shiny::shinyApp(app$ui, app$server)

```

# Using Rstudio via Nectar
***optional***
| setting | Description                                                                    |
|------|----------------------------------------------------------------------------------------|
|Host  |	use the instance IP address, or the hostname [hostname].[project].cloud.edu.au  |
|Login |	use the username you created in the Configure Application dialog                |
