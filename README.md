<b><br>R Package "gilmour"<br></b>

Contact information: josef.dolejs@uhk.cz    ;   djosef@post.cz  <br>

R package available:<br>
https://cran.r-project.org/web/packages/gilmour/index.html
<br>

GNU GENERAL PUBLIC LICENSE<br>
Version 3, 29 June 2007<br>
 Copyright (C) 2007 Free Software Foundation, Inc. <https://fsf.org/><br>
 Everyone is permitted to copy and distribute verbatim copies<br>
 of this license document, but changing it is not allowed.<br><br>

<b> Motivation and significance </b>

Several methods may be found for selecting a subset of regressors from a set of k candidate variables in multiple linear regression. One possibility is to evaluate all possible regression models and comparing them using Mallows's Cp statistic (Cp). For a particular submodel with p parameters, the Cp statistic is defined as:

<img width="120" height="30" alt="Eq1" src="https://github.com/user-attachments/assets/69f63b55-e9d6-4db4-87e3-f9d02b7304eb" /> 

where SSEₚ is the sum of squared errors for the submodel, and MSE is the mean square error from the full model with all possible regressors, used as an estimate of the error variance σ².
Gilmour proposed an adjusted version of the Cp statistic to improve model comparison, given by:

<img width="170" height="45" alt="Eq2" src="https://github.com/user-attachments/assets/40c99fda-054c-4243-ad2d-d5142a875c1e" />

where k is the number of regressors in the full model, and p - 1 is the number of regressors in the submodel (thus, p includes the intercept term).

[S.G. Gilmour, The interpretation of Mallows's Cp-statistic, The Statistician 45(1) (1996) 49 56, https://doi.org/10.2307/2348411]

For example, in the case of the trivial model (i.e., a model with only an intercept), the adjusted statistic is:

<img width="350" height="30" alt="EQ3" src="https://github.com/user-attachments/assets/d2ff2911-c8c1-4994-bc03-9efeb4eee8c4" />

where the function var() of sample variation is used for calculation of sum of squares in the trivial model and MSE is estimation of σ².

<b> Software description </b>
To use the package, R must be installed on the user’s device (PC, macOS, or Linux).
[The Comprehensive R Archive Network," Download and Install R",
https://cran.r-project.org/]

The package can be installed, for example, by issuing the command:
<br><b> install.packages("gilmour") </b><br>
After each start of R and after this installation of the package, the following the standard command should be used:
<br><b> library(gilmour) </b><br>
The gilmour package enables the identification of a suitable combination of regressors using the <b>"final_model()"</b> function. This method is particularly appropriate in situations where the number of observations is small but the number of potential regressors is large. However, it is important to note that the input data may have limited informational value, and the significance of the resulting model should not be overestimated. Nonetheless, if numerical predictors are available to explain a single numerical variable, the package provides a straightforward and user-friendly solution. Additionally, the <b>"submodels()"</b> function, which generates all combinations of regressors, can be effectively used in conjunction with other methods.

<br><b>Software architecture: <br></b>
The procedure consists of two main steps (more details may be found in help):
<br><b>Initial Selection:</b> Adjusted C̄p values are calculated for all possible combinations of regressors. The submodel with the minimum C̄p value is selected and labeled as ModelMin.
<br><b>Reduction and Testing:</b> All submodels nested within ModelMin are considered. Among them, the submodel with the lowest C̄p is selected as a new candidate. A hypothesis test is then performed where the null hypothesis states that ModelMin provides a better fit than the candidate submodel. If the null hypothesis is not rejected, ModelMin is accepted as the final model. If the null hypothesis is rejected, the candidate submodel becomes the new ModelMin, and the process is repeated with submodels nested within this new model. The procedure continues iteratively until a null hypothesis is not rejected or until the so-called trivial model (i.e., a model using only the arithmetic mean) is reached.
The package returns full regression results for: 
<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the full model,
<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the selected ModelMin, and
<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the resulting final model.
<br>Additionally, it outputs Cp values for all submodels, enabling users to easily generate a adjusted C̄p vs. p plot (where p is the number of regressors+1), as recommended by Gilmour. Diagram of the first step "Initial Selection" is in Figure 1, while the whole process is in Figure 2. Figure 1 is also diagram of the function "submodels()" and Figure 2 is diagram of the function "final_model()". For example, 1023 submodels were found for standard R data "mtcars", 511 submodels for data from the original study and 4,095 submodels for illustrative data T1 with 12 regressors.

<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Figure 1 Diagram of the first step<br> 
<img width="500" height="1000" alt="Figure1" src="https://github.com/user-attachments/assets/c4282df3-f493-4c6c-9ad4-df338e284d46" />

<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Figure 1 Diagram of the second step<br> 
<img width="500" height="1000" alt="Figure2" src="https://github.com/user-attachments/assets/30072e16-aa8a-4d88-a658-6a1c444ef599" />

<br><b>Software functionalities: <br></b>
The two functions submodels(d) and final_model(d) share the same input argument d, which must be a table in the data.frame format. The first column of this data frame should contain the response (dependent) numerical variable (y), while the remaining columns should contain the explanatory (independent) numerical variables (regressors). By default, each row represents a single observation. The number of observations (n) should be higher than the number of regressors (k) plus three (n>k+3).<br>
The functions ignore column names (colnames) and row names (rownames); however, the position of the first column is critical and must always contain the response variable.<br>
There are several methods for identifying a suitable resulting model in multiple linear regression, as discussed, for example, in studies [2–5]. The method proposed by Gimour is demanding, particularly in terms of constructing all possible submodels for all combinations of regressors. The presented software automates this process in the first step, as shown in Figure 1.
The subsequent phase generally involves the implementation of several tests to identify the final nested model, as illustrated in Figure 2. The user creates a table in data.frame format, where the columns represent the variables and the first column corresponds to the response variable. The functions submodels(d) and final_model(d) then provide the complete results.<br>
The software is implemented as a standard package within the R system, which makes it possible to further extend it or incorporate it into subsequent packages. It also enables easier comparison with other methods.<br>
It can be assumed that this package removes the main barrier to the application of this method for many users, namely the complicated construction and evaluation of the characteristics of a large number of submodels. Since the software is developed within the R system, commercial aspects are not relevant.<br>
<br><b>Illustrative examples<br></b>
The Gilmour package does not require any additional packages to be installed. However, to view the examples clearly in a browser, it is necessary to install the knitr package.<br>
The package includes nine illustrative datasets. Additionally, the standard R dataset "mtcars" is used as a tenth example in the help documentation (see: mtcars dataset).<br>
Some example datasets have no substantive meaning and are included purely to demonstrate technical capabilities of the functions. However, three of the datasets were used in previously published studies and carry practical relevance.<br>
The input data should be provided as a data.frame, where the first column contains the numerical dependent variable, and the subsequent columns contain the regressors. <br>
Standard R data "mtcars" may be used as the first simple illustration. In this dataset, the first column (mpg, representing car fuel consumption) is here used as the response variable. The following statements show functionality of the package. <br>
<br><b>d=mtcars ; MyResults=final_model(d)<br></b>
This is what the output looks like when applying the final_model(d) function:<br><i>
[1] "Number of regressors in model_min  3"<br>
[1] "model_min:  y=x5+x6+x8"<br>
[1] "Cq+1: -0.634206365802602"<br>
[1] "The starting value of q for the test of submodel is:   3"<br>
[1] "For q = 3: Ho is rejected, new submodel goes to the next test."<br>
[1] "For q = 2: the End, Ho is not rejected, the resulting model is: y=x5+x6+const.  F=11.79722 Cq+1 = 0.98767"<br>
[1] "Final value of q is :  2"
</i><br>

This is how the basic graph of the Gilmour method may be displayed, where in the first step the model with the minimum value of the adjusted statistic C̄p is searched.<br>

<b>yCp= as.numeric(MyResults$submodels[,3]) <br>
xp= as.numeric(MyResults$submodels[,2])<br>
ymin= ifelse(min(yCp)<0, 1.1* min(yCp), 0.9* min(yCp))<br>
YRange=c( ymin ,1.5*max(xp)) ; FinalCp= MyResults$FinalCp<br>
plot(yCp ~ xp, xlab="Number of Parameters in Submodel",ylab="", ylim=YRange ,<br>
 col=ifelse( ( round(yCp,4)== round(min(yCp),4)| round(yCp,4)== round(FinalCp,4) ), "red", "darkblue")  )<br>
lines(xp, xp, col="red")<br>
mtext(bquote(paste( bar(C) , "p")), side=2, line=3, padj=1, cex=1.2)<br>
mtext(bquote(paste("All Submodels: ",bar(C),"p ~ p")), side=3, line=3, padj=1, cex=1.2)<br></b>


<br>The resulting plot is in Figure 3. 

<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Figure 3 Relationship C̄p~p for all submodels calculated for data "mtcars".<br> 
<img width="600" height="450" alt="Figure3" src="https://github.com/user-attachments/assets/d19365c1-eeb4-4b5e-a52d-e4c0ac849e5d" />
<br><i>Notes: red circles corresponds to ModelMin and FinalModel.<br></i>

The following commands show additional outputs for the full model and for the final model.<br>
<b>MyResults$full_model<br>
summary(MyResults$full_model_results)<br>
MyResults$final_model <br>
summary(MyResults$final_model_results) <br></b>
Information about the model "ModelMin" with the minimum value of the adjusted C̄p characteristic can be obtained as follows:<br>
<b>MyResults$model_min <br>
summary(MyResults$model_min_results) <br></b>
This submodel can be identical to the final model and, in principle, to the full model. <br>
The next 9 illustrative tables may be also obtained using the examples() function:<br>
<b>examples= examples()</b><br>
The second illustrative data "Gilmour9p" were taken from the original study by Gilmour.<br>
The data contains 9 predictors and 24 observations (House price data).<br>
The outputs of the functions "submodels()" and "final_model()" are identical to the results in the original work.<br>

<b>d= examples$Gilmour9p ; MyResults=final_model(d)<br></b><i>
[1] "Number of regressors in model_min  2"<br>
[1] "model_min:  y=x1+x2"<br>
[1] "Cq+1: -0.248045011348888"<br>
[1] "The starting value of q for the test of submodel is:   2"<br>
[1] "For q = 2: Ho is rejected, new submodel goes to the next test."<br>
[1] "For q = 1 : the end, Ho is not rejected and the resulting model is: y=x1+constant, F = 72.17679 Cq+1 = 0.89705"<br>
[1] "Final value of q is :  1"<br></i>

The following statements show plot, where in the first step the model with the minimum value of the adjusted statistic C̄p is searched.
<b>yCp= as.numeric(MyResults$submodels[,3]) ; xp= as.numeric(MyResults$submodels[,2])<br>
ymin= ifelse(min(yCp)<0, 1.1* min(yCp), 0.9* min(yCp))<br>
YRange=c( ymin ,1.5*max(xp)) ; FinalCp= MyResults$FinalCp<br>
plot(yCp ~ xp, xlab="Number of Parameters in Submodel",ylab="", ylim=YRange ,<br>
 col=ifelse( ( round(yCp,4)== round(min(yCp),4)| round(yCp,4)== round(FinalCp,4) ), "red", "darkblue")  )<br>
lines(xp, xp, col="red")<br>
mtext(bquote(paste( bar(C) , "p")), side=2, line=3, padj=1, cex=1.2)<br>
mtext(bquote(paste("All Submodels: ",bar(C),"p ~ p")), side=3, line=3, padj=1, cex=1.2)<br></b>

The resulting plot is in Figure 4.<br>
<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Figure 4 Relationship C̄p~p for all submodels calculated for data "Gilmour9p".<br>
<img width="600" height="450" alt="Figure4" src="https://github.com/user-attachments/assets/c4d6398e-301b-4bf3-b81c-fc811a3f98c4" />
<br><i>Notes: red circles corresponds to ModelMin and FinalModel<br></i>

The same commands as was used for data mtcars and Gilmour9p may be used for all other tables.<br>
Two examples, Parks5p and Patents5p, were taken from published studies.<br>
The remaining examples demonstrate all other possible situations that may occur.<br>

