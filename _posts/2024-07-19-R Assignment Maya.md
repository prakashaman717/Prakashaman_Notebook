
**R-Programming**

R is a programming language for statistical computing and data visualization. It has been widely used in the fields of data mining, bioinformatics, and data analysis.

*Assignment* 

In order to the assigned work for the assignment follwing data and methods were used in R :- 

1. Data :- Photsurvery Meta Data (Unorganised),Photosurvey Processed Data
2. Source :- Assigned and Moodle(Uploaded), Collected at Moris Kahn Research Centre,Sdot Yam, Israel
3. Coding Platform :- Mac Down 
4. Code Source :- Methods ggplot Exerice R (Research Methods Course, Dr. Maya Lalzar,Head,Bioinformatics Unit,Haifa Univerisity)
5. Software :- R and R Visual Studio (V 4.4.1)

I have selected Bryoza and Cnidaria becuase both are predominantly found together with each other and are sometimes symbotic.So, I analyzed two groups from the processed data and found that : -

They have a positive co-relation.So, I have selected both of them out of all.
![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Co-relation%20between%20Bryptha%20and%20Cnidaris.png)

Barlet test was runned and got the following answer :- 
![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Barlett%20Test.png)

The graph between the Bryozoa and season was plotted with respective of both the sites: -
![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Bryozo%20and%20Season%20and%20SITE.png)

Then after the co-relation graph was plotted between Cnadaria and Bryozoa :- 
![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Cnadirri%20anad%20Bryozoa.png)







The whole R script is copied below :- 


  setwd("/Users/amanprakash/Desktop/R Studio Research Methods") # set working directory

  #load the metadata- NOTE: we work at the transect level.
photo_metadata=read.csv("Photosurvey_metadata.csv", header=TRUE, row.names=1, stringsAsFactors = TRUE)
str(photo_metadata)

  #load the processed and filtered coverage data by taxonomic group
photosurvey_coverage=read.csv("Photosurvey_processed_data.csv", header=TRUE, row.names=1, stringsAsFactors = TRUE)
str(photosurvey_coverage)

  #make sure the order of trnasect_IDs is the same in the data and the metadata
ord=match(row.names(photosurvey_coverage), row.names(photo_metadata))
photo_metadata=photo_metadata[ord,]

  #merge metadata and coverage values

  Photosurvey=cbind(photo_metadata, photosurvey_coverage)
  str(Photosurvey)

library(ggplot2)

ggplot(Photosurvey, aes(x=site, y=Bryozoa))+
  geom_boxplot(outlier.shape = NA)+
  geom_point(aes(color=as.factor(depth)),position=position_jitter(width=0.4))+
  theme_light()

#now test

#1. test homogeneity of variance

bartlett.test(Photosurvey$Bryozoa~Photosurvey$site)

#2) test difference in Other levels

kruskal.test(Bryozoa~site, data=Photosurvey)

#now let's moove to season

ggplot(Photosurvey, aes(x=season, y=Bryozoa, fill=site))+
  geom_boxplot(outlier.shape = NA)+
  geom_point(position=position_jitterdodge(jitter.width = 0.2), size=1)+
  theme_light()+
  facet_grid(.~depth)

###now let's test for all factors together- non-parametric anova
library(ARTool)
m=art(Bryozoa~site+as.factor(depth)+season+site*as.factor(depth)*season, data=Photosurvey)
anova(m)


###let's examine the sampler....

ggplot(Photosurvey, aes(x=site_depth, y=Cnidaria))+
  geom_boxplot(outlier.shape = NA)+
  geom_point(aes(color=as.factor(depth)),position=position_jitter(width=0.4))+
  theme_light()


#1. test homogeneity of variance

bartlett.test(Photosurvey$Bryozoa~Photosurvey$site_depth)

###what is the relatedness between coverage by different groups? we will do correlations

library(Hmisc)
str(Photosurvey)

#variables 10 to 22 have the measurements

corrs=rcorr(as.matrix(Photosurvey[0:5 ,10:22]), type="spearman")

#we need a function that will take the square matrix and turn it to long format.

flattenCorrMatrix <- function(cormat, pmat) {
  ut <- upper.tri(cormat)
  data.frame(
    row = rownames(cormat)[row(cormat)[ut]],
    column = rownames(cormat)[col(cormat)[ut]],
    cor  =(cormat)[ut],
    p = pmat[ut]
  )
}

#now we apply this function to our data
corrs_flat=flattenCorrMatrix(corrs$r, corrs$P)

#add p value adjustment using the Benjamini-Hochber method (BH)
corrs_flat$p.adj=p.adjust(corrs_flat$p, "BH")

write.csv(corrs_flat, "Taxonomic_groups_spearman.csv")

#now plot your correlation of interest e.g., my taxon of interest and live coverage
str(Photosurvey)

ggplot(Photosurvey, aes(x=Bryozoa, y=Cnidaria))+
  geom_point()+
  geom_smooth(method='lm')+
  theme_light()

