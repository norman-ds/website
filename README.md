
<!-- README.md is generated from README.Rmd. Please edit that file  -->

# Blogdown website

A github/blogdown website with
hugo

<!-- badges: start -->

![Render2README](https://github.com/norman-ds/website/workflows/Render2README/badge.svg)
<!-- badges: end -->

## DevOps

This project follows a continuous process approach covering software
development and IT operations. All scripts are written in
[R](https://www.r-project.org). The
[Docker](https://hub.docker.com/r/rocker/verse) container system is used
for reproducible software development. The versioning system is
[GitHub](https://help.github.com/en/actions/building-and-testing-code-with-continuous-integration),
GitHub *Action* for CI/CD and [Netlify](https://www.netlify.com) as
Webserver.

The RStudio IDE is launched in a docker and accessed via the browser
(<http://localhost:8787>).

    docker run --name website -d -p 8787:8787 -v $(pwd):/home/rstudio -e PASSWORD=pwd rocker/verse:3.6.3

Next we need to install the blogdown package in R and the static site
generator Hugo (<https://gohugo.io>) version *0.74.3*.

    install.packages('blogdown')
    blogdown::install_hugo(version="0.74.3")

To use the theme *data-science* from [Amber
Thomas](https://github.com/ProQuestionAsker/websiteSource), you’ll need
to download all the files in that directory with *svn*, since *git*
won’t work for this target. Be careful with the hugo version, since it
was written for an older version
    *0.54.0*.

    svn export https://github.com/ProQuestionAsker/websiteSource/trunk/themes/data-science 
    
    # the static files too
    svn export https://github.com/ProQuestionAsker/websiteSource/trunk/static

The docker image of *rocker/verse* has the R version 3.6.3 and some
packages installed and saves us from having to install them again.

``` r
sessionInfo()
#> R version 3.6.3 (2020-02-29)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: Debian GNU/Linux 10 (buster)
#> 
#> Matrix products: default
#> BLAS/LAPACK: /usr/lib/x86_64-linux-gnu/libopenblasp-r0.3.5.so
#> 
#> locale:
#>  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
#>  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
#>  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=C             
#>  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
#>  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
#> [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> loaded via a namespace (and not attached):
#>  [1] compiler_3.6.3  magrittr_1.5    tools_3.6.3     htmltools_0.4.0
#>  [5] yaml_2.2.1      Rcpp_1.0.4.6    stringi_1.4.6   rmarkdown_2.1  
#>  [9] knitr_1.28      stringr_1.4.0   xfun_0.13       digest_0.6.25  
#> [13] rlang_0.4.5     evaluate_0.14
```

The following is a list of the few packages used (with version) …

``` r
libs <- c("blogdown")
ip <- installed.packages(fields = c("Package", "Version"))
ip <- as.data.frame(ip)
ip <- ip[ip[,c("Package")] %in% libs,]
paste(ip[,c("Package")],ip[,c("Version")])
#> [1] "blogdown 0.18"
```

… and the hugo version

``` r
blogdown::hugo_version()
#> [1] '0.74.3'
```

## Local Testing

Since Rstudio runs in a container a simple extra web server is used for
testing static websites. To start from terminal a HTTPServer go to the
directory *public* and type:

    python -m SimpleHTTPServer 8000
