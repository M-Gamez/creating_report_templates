# Custom Reporting Functions

Below, we detail how to convert the following [R Epidemics Member
Gallery](https://www.repidemicsconsortium.org/people/) into RMD format
by replicating the existing gallery template using a custom R function
to build the duplicate gallery.

## Step 1: Create a New RMD File

Create a new RMD file using the below YAML settings.

    ---
    title: 'Page Title'
    output:
       html_document:
         theme: readable
         df_print: paged
         highlight: zenburn
         toc: true
    ---

## Step 2: Member Data Frame

Create a data frame for members with vectors for each information type
(WEBSITE, NAME, etc) and manually add items for team members and use an
empty string as a placeholder (““) if a member is missing information in
one of the fields. <br> <br> Bind the vectors together into a data frame
using `data.frame()` function.

    member_list <- list(
                        FIRST_NAME = c("Cici",
                                       "Matthijs",
                                       "Mahendra",
                                       "Sangeeta"),
                        LAST_NAME = c("Bauer", 
                                      "Berends", 
                                      "Bhandari", 
                                      "Bhatia"),
                        WEBSITE = c( "https://cicibauer.netlify.com/", 
                                "https://www.rug.nl/staff/m.s.berends/",
                                "https://%20www.ainqa.com/", 
                                "https://sangeetabhatia03.github.io/"),
                        IMG = c( "https://raw.githubusercontent.com/Watts-College/paf-514-template/main/labs/images/avatar-01.png", 
                                 "https://raw.githubusercontent.com/Watts-College/paf-514-template/main/labs/images/avatar-02.png", 
                                 "https://raw.githubusercontent.com/Watts-College/paf-514-template/main/labs/images/avatar-03.png", 
                                 "https://raw.githubusercontent.com/Watts-College/paf-514-template/main/labs/images/avatar-04.png"),
                        DESC = c("Academic faculty (assistant professor) - Bayesian spatiotemporal modeling, Department of Biostatistics and Data Science, University of Texas Health Science Center in Houston, United States",
                                 "Infectious Disease Epidemiology, University of Groningen, Netherlands", 
                                 "Head Intelligent Automation , Predictive Modelling , Ensemble Forecast , Location Based Intelligence , Python / R, AINQA, India", 
                                 "Modeller and software developer contributing packages for outbreak analysis using digital surveillance data., Imperial College London, UK., NA"),
                        GITHUB = c( "https://github.com/cicibauer", 
                                    "https://github.com/msberends", 
                                    "", 
                                  "https://github.com/sangeetabhatia03"), 
                        TWITTER = c("", 
                                    "https://twitter.com/msberends", 
                                    "",
                                    "https://twitter.com/sangeeta0312")
                        )
    # Create Member Data Frame
    member_df <- as.data.frame(member_list)

    # Create combined name column
    member_df$NAME <- paste(member_df$FIRST_NAME, member_df$LAST_NAME)

    # Move combined name column to font of data set
    member_df <- member_df %>% 
                    select(NAME, everything())

    # Sort df by last name
    member_df <- member_df[order(member_df$LAST_NAME),]

    print(member_df)

### Useful HTML with variables

    <a href="WEBSITE"><img src="IMG" class="item-img"></a>

    <h4 class="item-name">NAME</h4>
    <div class="item-desc">DESC</div>

    <div class="item-links">

       <a class="item-link" href="WEBSITE" title="Website">
       <svg><path d="..."/></svg>   /* --- abbreviated fa.home svg --- */
       </a>

       <a class="item-link" href="GITHUB" title="GitHub">
       <svg><path d="..."/></svg>   /* --- abbreviated fa.github svg --- */
       </a>

       <a class="item-link" href="TWITTER" title="Twitter">
       <svg><path d="..."/></svg>   /* --- abbreviated fa.twitter svg --- */
       </a>

    </div>

## Step 3: Build\_circle Function

Write an R function called build\_circle() that will create a single
member profile for the team gallery page.

### Member Elements:

-   WEBSITE - personal website URL
-   IMG - full URL of profile photo of a team member
-   NAME - team member full name
-   DESC - one sentence bio
-   GITHUB - URL of personal GitHub page
-   TWITTER - Twitter URL

<!-- -->

    # build_circle function

    build_circle <- function(WEBSITE="", IMG="", NAME="", DESC="", GITHUB="", TWITTER="")
      {
        
      # beginning of code
      cat( '<div class="list-circles-item">')
      cat( '\n') # line break
      cat( '\n') # line break
      
      # member name & image
      if (WEBSITE != ""){
      cat( paste0(' <a href="', WEBSITE, '">') )
      cat( '\n')  
      cat( paste0(' <img src="', IMG, '" class="item-img"></a>') )
      } else {
        cat( paste0(' <img src="', IMG, '" class="item-img">') )
      }
      cat( '\n') 
      cat( '\n') 
      if (NAME != ""){
      cat( paste0(' <h4 class="item-name">', NAME, '</h4>') )
      } 
      cat( '\n') 
       
      # description
      if(DESC != ""){
      cat( paste0('  <div class="item-desc">',  DESC,'</div>'))
      } else {
      cat( paste0('  <div class="item-desc">',  "Bio coming soon!",'</div>'))
      }
      cat( '\n')  
      cat( '\n') 
      
      # links / fontawesome icons
      cat( '   <div class="item-links">')
      cat( '\n') 
      cat( '\n') 
      
      ## home page
      if(WEBSITE != ""){
      cat( paste0('   <a class=item-link" href="', WEBSITE, '" title="Website">'), fontawesome::fa(name="home"), '</a>' )
      cat( '\n') 
      cat( '\n') 
      }
      
      ## github
      if(GITHUB != ""){
      cat( paste0('   <a class=item-link" href="', GITHUB, '" title="Github">'), fontawesome::fa(name="github"), '</a>' )
      cat( '\n') 
      cat( '\n') 
      }
      
      ##twitter
      if(TWITTER != ""){
      cat( paste0('   <a class=item-link" href="', TWITTER, '" title="Twitter">'), fontawesome::fa(name="twitter"), '</a>' )
      cat( '\n') 
      cat( '\n') 
      }
      cat( '   </div>')
      
      # end of code
      cat( '\n') 
      cat( '\n') 
      cat('</div>')
    }

## Step 4: Loop Through R Epidemics Consortium Members

Create a loop to generate profile sections for team members using
`cat()` function.

<a href="https://cicibauer.netlify.com/">
<img src="https://raw.githubusercontent.com/Watts-College/paf-514-template/main/labs/images/avatar-01.png" class="item-img"></a>

<h4 class="item-name">
Cici Bauer
</h4>

Academic faculty (assistant professor) - Bayesian spatiotemporal
modeling, Department of Biostatistics and Data Science, University of
Texas Health Science Center in Houston, United States

<a class=item-link" href="https://cicibauer.netlify.com/" title="Website">
<svg aria-hidden="true" role="img" viewBox="0 0 576 512" style="height:1em;width:1.12em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M575.8 255.5c0 18-15 32.1-32 32.1h-32l.7 160.2c0 2.7-.2 5.4-.5 8.1V472c0 22.1-17.9 40-40 40H456c-1.1 0-2.2 0-3.3-.1c-1.4 .1-2.8 .1-4.2 .1H416 392c-22.1 0-40-17.9-40-40V448 384c0-17.7-14.3-32-32-32H256c-17.7 0-32 14.3-32 32v64 24c0 22.1-17.9 40-40 40H160 128.1c-1.5 0-3-.1-4.5-.2c-1.2 .1-2.4 .2-3.6 .2H104c-22.1 0-40-17.9-40-40V360c0-.9 0-1.9 .1-2.8V287.6H32c-18 0-32-14-32-32.1c0-9 3-17 10-24L266.4 8c7-7 15-8 22-8s15 2 21 7L564.8 231.5c8 7 12 15 11 24z"/></svg>
</a>

<a class=item-link" href="https://github.com/cicibauer" title="Github">
<svg aria-hidden="true" role="img" viewBox="0 0 496 512" style="height:1em;width:0.97em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"/></svg>
</a>

<a href="https://www.rug.nl/staff/m.s.berends/">
<img src="https://raw.githubusercontent.com/Watts-College/paf-514-template/main/labs/images/avatar-02.png" class="item-img"></a>

<h4 class="item-name">
Matthijs Berends
</h4>

Infectious Disease Epidemiology, University of Groningen, Netherlands

<a class=item-link" href="https://www.rug.nl/staff/m.s.berends/" title="Website">
<svg aria-hidden="true" role="img" viewBox="0 0 576 512" style="height:1em;width:1.12em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M575.8 255.5c0 18-15 32.1-32 32.1h-32l.7 160.2c0 2.7-.2 5.4-.5 8.1V472c0 22.1-17.9 40-40 40H456c-1.1 0-2.2 0-3.3-.1c-1.4 .1-2.8 .1-4.2 .1H416 392c-22.1 0-40-17.9-40-40V448 384c0-17.7-14.3-32-32-32H256c-17.7 0-32 14.3-32 32v64 24c0 22.1-17.9 40-40 40H160 128.1c-1.5 0-3-.1-4.5-.2c-1.2 .1-2.4 .2-3.6 .2H104c-22.1 0-40-17.9-40-40V360c0-.9 0-1.9 .1-2.8V287.6H32c-18 0-32-14-32-32.1c0-9 3-17 10-24L266.4 8c7-7 15-8 22-8s15 2 21 7L564.8 231.5c8 7 12 15 11 24z"/></svg>
</a>

<a class=item-link" href="https://github.com/msberends" title="Github">
<svg aria-hidden="true" role="img" viewBox="0 0 496 512" style="height:1em;width:0.97em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"/></svg>
</a>

<a class=item-link" href="https://twitter.com/msberends" title="Twitter">
<svg aria-hidden="true" role="img" viewBox="0 0 512 512" style="height:1em;width:1em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M459.37 151.716c.325 4.548.325 9.097.325 13.645 0 138.72-105.583 298.558-298.558 298.558-59.452 0-114.68-17.219-161.137-47.106 8.447.974 16.568 1.299 25.34 1.299 49.055 0 94.213-16.568 130.274-44.832-46.132-.975-84.792-31.188-98.112-72.772 6.498.974 12.995 1.624 19.818 1.624 9.421 0 18.843-1.3 27.614-3.573-48.081-9.747-84.143-51.98-84.143-102.985v-1.299c13.969 7.797 30.214 12.67 47.431 13.319-28.264-18.843-46.781-51.005-46.781-87.391 0-19.492 5.197-37.36 14.294-52.954 51.655 63.675 129.3 105.258 216.365 109.807-1.624-7.797-2.599-15.918-2.599-24.04 0-57.828 46.782-104.934 104.934-104.934 30.213 0 57.502 12.67 76.67 33.137 23.715-4.548 46.456-13.32 66.599-25.34-7.798 24.366-24.366 44.833-46.132 57.827 21.117-2.273 41.584-8.122 60.426-16.243-14.292 20.791-32.161 39.308-52.628 54.253z"/></svg>
</a>

<a href="https://%20www.ainqa.com/">
<img src="https://raw.githubusercontent.com/Watts-College/paf-514-template/main/labs/images/avatar-03.png" class="item-img"></a>

<h4 class="item-name">
Mahendra Bhandari
</h4>

Head Intelligent Automation , Predictive Modelling , Ensemble Forecast ,
Location Based Intelligence , Python / R, AINQA, India

<a class=item-link" href="https://%20www.ainqa.com/" title="Website">
<svg aria-hidden="true" role="img" viewBox="0 0 576 512" style="height:1em;width:1.12em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M575.8 255.5c0 18-15 32.1-32 32.1h-32l.7 160.2c0 2.7-.2 5.4-.5 8.1V472c0 22.1-17.9 40-40 40H456c-1.1 0-2.2 0-3.3-.1c-1.4 .1-2.8 .1-4.2 .1H416 392c-22.1 0-40-17.9-40-40V448 384c0-17.7-14.3-32-32-32H256c-17.7 0-32 14.3-32 32v64 24c0 22.1-17.9 40-40 40H160 128.1c-1.5 0-3-.1-4.5-.2c-1.2 .1-2.4 .2-3.6 .2H104c-22.1 0-40-17.9-40-40V360c0-.9 0-1.9 .1-2.8V287.6H32c-18 0-32-14-32-32.1c0-9 3-17 10-24L266.4 8c7-7 15-8 22-8s15 2 21 7L564.8 231.5c8 7 12 15 11 24z"/></svg>
</a>

<a href="https://sangeetabhatia03.github.io/">
<img src="https://raw.githubusercontent.com/Watts-College/paf-514-template/main/labs/images/avatar-04.png" class="item-img"></a>

<h4 class="item-name">
Sangeeta Bhatia
</h4>

Modeller and software developer contributing packages for outbreak
analysis using digital surveillance data., Imperial College London, UK.,
NA

<a class=item-link" href="https://sangeetabhatia03.github.io/" title="Website">
<svg aria-hidden="true" role="img" viewBox="0 0 576 512" style="height:1em;width:1.12em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M575.8 255.5c0 18-15 32.1-32 32.1h-32l.7 160.2c0 2.7-.2 5.4-.5 8.1V472c0 22.1-17.9 40-40 40H456c-1.1 0-2.2 0-3.3-.1c-1.4 .1-2.8 .1-4.2 .1H416 392c-22.1 0-40-17.9-40-40V448 384c0-17.7-14.3-32-32-32H256c-17.7 0-32 14.3-32 32v64 24c0 22.1-17.9 40-40 40H160 128.1c-1.5 0-3-.1-4.5-.2c-1.2 .1-2.4 .2-3.6 .2H104c-22.1 0-40-17.9-40-40V360c0-.9 0-1.9 .1-2.8V287.6H32c-18 0-32-14-32-32.1c0-9 3-17 10-24L266.4 8c7-7 15-8 22-8s15 2 21 7L564.8 231.5c8 7 12 15 11 24z"/></svg>
</a>

<a class=item-link" href="https://github.com/sangeetabhatia03" title="Github">
<svg aria-hidden="true" role="img" viewBox="0 0 496 512" style="height:1em;width:0.97em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"/></svg>
</a>

<a class=item-link" href="https://twitter.com/sangeeta0312" title="Twitter">
<svg aria-hidden="true" role="img" viewBox="0 0 512 512" style="height:1em;width:1em;vertical-align:-0.125em;margin-left:auto;margin-right:auto;font-size:inherit;fill:currentColor;overflow:visible;position:relative;"><path d="M459.37 151.716c.325 4.548.325 9.097.325 13.645 0 138.72-105.583 298.558-298.558 298.558-59.452 0-114.68-17.219-161.137-47.106 8.447.974 16.568 1.299 25.34 1.299 49.055 0 94.213-16.568 130.274-44.832-46.132-.975-84.792-31.188-98.112-72.772 6.498.974 12.995 1.624 19.818 1.624 9.421 0 18.843-1.3 27.614-3.573-48.081-9.747-84.143-51.98-84.143-102.985v-1.299c13.969 7.797 30.214 12.67 47.431 13.319-28.264-18.843-46.781-51.005-46.781-87.391 0-19.492 5.197-37.36 14.294-52.954 51.655 63.675 129.3 105.258 216.365 109.807-1.624-7.797-2.599-15.918-2.599-24.04 0-57.828 46.782-104.934 104.934-104.934 30.213 0 57.502 12.67 76.67 33.137 23.715-4.548 46.456-13.32 66.599-25.34-7.798 24.366-24.366 44.833-46.132 57.827 21.117-2.273 41.584-8.122 60.426-16.243-14.292 20.791-32.161 39.308-52.628 54.253z"/></svg>
</a>

## Step 5: Add Custom CSS to Replicate Gallery Style

Adding list-circles CSS items to RMD code to stylize elements.

    <style>
    /* --- css elements here --- */
    /* --- Lists of circles --- */

    div { display: block } 
      
    .list-circles {
      text-align: center;
    }

    @media only screen and (min-width: 1200px) {
      .list-circles {
        width: 150%;
        margin-left: -25%;
      }
    }


    .list-circles-item {
      display: inline-block;
      width: 240px;
      vertical-align: top;
      margin: 0;
      padding: 20px;
    }

    .list-circles-item:hover {
      background: #fafafa;
    }

    .list-circles-item .item-img {
      max-width: 200px;
      height: 200px;
      -webkit-border-radius: 50%;
      -moz-border-radius: 50%;
      border-radius: 50%;
      border: 1px solid #777;
    }

    .list-circles-item .item-desc {
      font-size: 16px;
      font-family: 'Open Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
    }

    .list-circles-item .item-links {
      margin-top: 10px;
      padding-right: 5px;
      display: flex; 
      justify-content: center; 
      align-items: center;
    }

    .list-circles-item .item-link {
      margin:0 3px;
      color: #314f96;
      text-decoration: none !important;
      padding-right: 10px;
      padding-left: 10px;
    }

    .list-circles-item .item-link:hover {
      color: #042265;
    }

    .svg-icon {
      display: inline-flex;
      align-self: center;
    }
    </style>

<style type="text/css">
<style>
/* --- css elements here --- */
/* --- Lists of circles --- */

div { display: block } 
  
.list-circles {
  text-align: center;
}

@media only screen and (min-width: 1200px) {
  .list-circles {
    width: 150%;
    margin-left: -25%;
  }
}


.list-circles-item {
  display: inline-block;
  width: 240px;
  vertical-align: top;
  margin: 0;
  padding: 20px;
}

.list-circles-item:hover {
  background: #fafafa;
}

.list-circles-item .item-img {
  max-width: 200px;
  height: 200px;
  -webkit-border-radius: 50%;
  -moz-border-radius: 50%;
  border-radius: 50%;
  border: 1px solid #777;
}

.list-circles-item .item-desc {
  font-size: 16px;
  font-family: 'Open Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

.list-circles-item .item-links {
  margin-top: 10px;
  padding-right: 5px;
  display: flex; 
  justify-content: center; 
  align-items: center;
}

.list-circles-item .item-link {
  margin:0 3px;
  color: #314f96;
  text-decoration: none !important;
  padding-right: 10px;
  padding-left: 10px;
}

.list-circles-item .item-link:hover {
  color: #042265;
}

.svg-icon {
  display: inline-flex;
  align-self: center;
}
</style>
</style>
