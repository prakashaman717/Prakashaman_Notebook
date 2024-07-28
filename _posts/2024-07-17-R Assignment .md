## Data Analysis using R ##

**R-Programming**

R is a programming language for statistical computing and data visualization. It has been widely used in the fields of data mining, bioinformatics, and data analysis.

*Assignment* 

In order to the assigned work for the assignment follwing data and methods were used in R :- 

1. Data :- Photsurvery Meta Data (Unorganised),Photosurvey Processed Data
2. Source :- Assigned and Moodle(Uploaded), Collected at Moris Kahn Research Centre,Sdot Yam, Israel
3. Coding Platform :- Mac Down 
4. Code Source :- Methods ggplot Exerice R (Research Methods Course, Dr. Maya Lalzar,Head,Bioinformatics Unit,Haifa Univerisity)
5. Software :- R and R Visual Studio (V 4.4.1)

I have selected Bryoza and Cnadaria 

Steps and Codes are as follows:- 

Step 1 :- Working Directory was set to the designetaed folder.
 
		setwd("/Users/amanprakash/Desktop/R Studio Research Methods")
Setp 2 :- Load the Metadata and Processed data and filtered the covergae data.we work at the transect level.

	photo_metadata=read.csv("Photosurvey_metadata.csv", header=TRUE, row.names=1, stringsAsFactors = TRUE) str(photo_metadata)
	
	photosurvey_coverage=read.csv("Photosurvey_processed_data.csv", header=TRUE, row.names=1, stringsAsFactors = TRUE) str(photosurvey_coverage)
	
Step 3 :- Kindly make sure the order of trnasect IDs is the same in the data and the metadata. 
		
	ord=match(row.names(photosurvey_coverage), row.names(photo_metadata))photo_metadata=photo_metadata[ord,]
Step 4 :- Now, merge the meta data and the coverage values.

	Photosurvey=cbind(photo_metadata, photosurvey_coverage) str(Photosurvey)
	
Step 5 :- Now load the packages of the library(ggplot2) and make a box plot of it. 

	library(ggplot2)ggplot(Photosurvey, aes(x=site, y=Other))+geom_boxplot(outlier.shape = NA)+ geom_point(aes(color=as.factor(depth)),position=position_jitter(width=0.2))+ theme_light()
	
Results:- 
![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/R-PLOT1.png)
	
Step 6 :- Now, we will do the different kinds of test:-

   (i) Test homogeneity of variance with Others and Site

		bartlett.test(Photosurvey$Other~Photosurvey$site)
Results :- 
![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Plot%20Result%201.png)

  (ii) Test difference in Others site with photosurvey.

		kruskal.test(Other~site, data=Photosurvey)
Results :- 

![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Plot%20Result%202.png)


  (iii) Now let's moove to season and will plot a boxplot with it.

		ggplot(Photosurvey, aes(x=season, y=Other, fill=site))+geom_boxplot(outlier.shape = NA)+geom_point(position=position_jitterdodge(jitter.width = 0.5), size=1)+theme_light()+facet_grid(.~depth)
		
Results :- 
![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Others%20Vs%20Season.png)


Step 7 :- Now we will test for all factors together in non-parametric anova

		library(ARTool)m=art(Other~site+as.factor(depth)+season+site*as.factor(depth)*season, data=Photosurvey)anova(m)
Results :- 

![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Annova%20Table.png)

Step 8 :- We will examine the sampler and try to make it more clear.

	ggplot(Photosurvey, aes(x=site_depth, y=Other))+ geom_boxplot(outlier.shape = NA)+ geom_point(aes(color=as.factor(depth)),position=position_jitter(width=0.2))+theme_light()

Results :- 

![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Rplot02.png)


Step 9 :- Now we will do the test homogeneity of variance.

	bartlett.test(Photosurvey$Other~Photosurvey$site_depth)
 
 Results :- 

 ![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Homogenity%201.png)

Step 10 :- Now we will do the test difference in coverage levels.

	kruskal.test(Other~site_depth, data=Photosurvey)
 
 Results :- 

  ![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Krushal%20Walk%20Wali.png)

Step 11 :- Now in order to do the co-relations or relatedness between coverage by different groups use the following code :- 

	library(Hmisc)
	str(Photosurvey)
	# variables 10 to 22 have the measurements
	corrs=rcorr(as.matrix(Photosurvey[0:5 ,10:22]), type="spearman")
 Results :- 
  ![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Variables.png)

Step 12 :- We need a function that will take the square matrix and turn it to long format.

	flattenCorrMatrix <- function(cormat, pmat) {
  	ut <- upper.tri(cormat)
  	data.frame(row = rownames(cormat)[row(cormat)[ut]],column = rownames(cormat)[col(cormat)[ut]],cor  =(cormat)[ut],p = pmat[ut])}

Step 13 :- Now we apply this function to our data.

	corrs_flat=flattenCorrMatrix(corrs$r, corrs$P)

Now, Add p value adjustment using the Benjamini-Hochber method (BH)

	corrs_flat$p.adj=p.adjust(corrs_flat$p, "BH")

Step 14 :- Now, write.csv(corrs_flat, "Alage.csv").

Result :- 

 ![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Algae%20CSV.png)

Step 15 :- Now we will plot our correlation of interest e.g., Algae and live coverage
	
 	str(Photosurvey)
	ggplot(Photosurvey, aes(x=Other, y=Algae))+geom_point()+ geom_smooth(method='lm')+ theme_light()

 Results :- 
 
  ![](https://github.com/prakashaman717/Prakashaman_Notebook/blob/main/images/Alage%20vs%20Others.png)
 



