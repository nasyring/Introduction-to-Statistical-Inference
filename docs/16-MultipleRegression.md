# Multiple Linear Regression

In this chapter we extend the simple linear regression model to accomodate multiple covariates $X_1, \ldots, X_p$ to better predict the response $Y$.  <br><br>



## The Multiple Linear Regression Model

Multiple linear regression is a model for predicting the behavior of a response variable $Y$ using several predictors (also called covariates or explanatory variables) $X_1, \ldots, X_p$ assumed to have some influence on the response.  In particular, the model is linear:
\[Y_i = \beta_0 + \beta_1 X_{1,i} + \beta_2 X_{2,i} + \cdots + \beta_p X_{p,i} + \epsilon_i\]
for $i=1, \ldots, n$ responses.  We assume the random residuals $\epsilon_i$ are a normal random sample with mean zero and constant variance $\sigma^2$.<br><br>

It is convenient to write the multiple linear regression model in matrix-vector form:
\[Y = X\beta+\epsilon\]
where $Y$ is the $n\times 1$ vector of response variables, $X$ is the $n \times (p+1)$ matrix consisting of an $n\times 1$ column vector of 1's and the $p$ columns of predictor variables, where $\beta$ is a $(p+1) \times 1$ vector of unknown coefficients, and where $\epsilon$ is the $n\times 1$ vector of random residuals.


## Point estimation

Just as in simple linear regression, we estimate $\beta$ using the method of least squares.  Define the observed residuals

\[e(\hat\beta) = Y_i - X_i^\top\hat\beta\]

where $\hat\beta$ is some particular value of the vector $\beta$ and $X_i$ is the $i^{th}$ row of the "design matrix" $X$ defined above.  Then, the sum of squared observed residuals (often called sum of squares errors) is
\[SSE = \sum_{i=1}^n \left(Y_i - X_i^\top\hat\beta\right)^2.\]
In matrix-vector notation, the SSE is an inner product (dot product): 
\[SSE = (Y-X\hat\beta)^\top (Y-X\hat\beta).\]

The method of least squares says that we should choose the estimator $\hat\beta$ to be the value minimizing the SSE.  And, just as in simple linear regression, we can compute this minimizer using calculus.  In multiple linear regression, the calculus is a bit more complicated, because we are dealing with vector rather than scalar quantities.  To compute the least squares estimator, begin by expanding SSE:
\begin{align*}
SSE &= (Y-X\hat\beta)^\top (Y-X\hat\beta)\\
& = Y^\top Y - Y^\top X\beta - \beta^\top X^\top Y + \hat\beta^\top X^\top X\beta
\end{align*}
Next, differentiate term by term, using the following facts from calculus: for $n \times 1$ vectors $a$ and $x$
\[\frac{\partial}{\partial x} a^\top x = a^\top;\]
and, for an $n\times n$ matrix $A$:
\[\frac{\partial}{\partial x}  x^\top A x = (A + A^\top) x.\]
Applying these rules, we obtain
\begin{align*}
\frac{\partial}{\partial \beta} SSE &= -Y^\top X - (Y^\top X)^\top + (X^\top X + X^\top X) \beta\\
& = 2X^\top X \beta - 2X^\top Y
\end{align*}
If $X^\top X$ is invertible, we may set this equation equal to the $(p+1)\times 1$ zero vector and solve, obtaining
\[\hat\beta = (X^\top X)^{-1}X^\top Y.\]
Just as in simple linear regression, our least squares estimator is unbiased:
\[E(\hat\beta) = E((X^\top X)^{-1}X^\top Y) = (X^\top X)^{-1}X^\top X\beta = \beta.\]
To estimate $\sigma^2$, we use (an adjustment to) the method of moments, 
\[\hat\sigma^2 = \frac{1}{n-(p+1)}SSE = \frac{1}{n-(p+1)}(Y - X\hat\beta)^\top(Y - X\hat\beta).\]
This estimator is also unbiased, i.e., $E(\hat\sigma^2) = \sigma^2$.

## Sampling Distributions

The estimated regression coefficients have a multivariate normal distribution due to the fact they are equal to linear combinations of the normally-distributed responses:
\[\hat\beta \sim N_{p+1}\left(\beta, \sigma^2 (X^\top X)^{-1}\right).\]
This means the least squares estimator is normally distributed, unbiased, and has covariance matrix $\sigma^2 (X^\top X)^{-1}$.  For example, $V(\hat\beta_j) = \sigma^2 (X^\top X)^{-1}_{j,j}$, which is the variance parameter $\sigma^2$ multiplies by the $j^{th}$ diagonal entry of the matrix $(X^\top X)^{-1}$.<br><br>

The variance estimator $\hat\sigma^2$ (properly scaled) has a Chi-Squared distribution:
\[\frac{[n-(p+1)]\hat\sigma^2}{\sigma^2}\sim \chi^2( n - p - 1).\]

Furthermore, the least squares estimator is independent of the variance estimator, which implies the Studentized estimated coefficients are $t-$distributed:
\[\frac{\hat\beta_j - \beta_j}{\sqrt{\hat\sigma^2 (X^\top X)^{-1}_{j,j}}}\sim t_{n-p-1}.\]


## Inference on regression coefficients

The Student's t sampling distribution noted above enables us to derive exact confidence intervals and hypothesis tests for individual regression coefficients.  A $100(1-\alpha)\%$ CI for $\beta_j$ is given by
\[(\hat\beta_j \pm t_{1-\alpha/2, n-p-1}\sqrt{\hat\sigma^2 (X^\top X)^{-1}_{j,j}}).\]
To test $H_0:\beta_j = 0$ versus $H_a:\beta_j\ne 0$ we reject the null hypothesis at level $\alpha$ if $|t| > t_{1-\alpha.2, n-p-1}$ where
\[t = \frac{\hat\beta_j - 0}{\sqrt{\hat\sigma^2 (X^\top X)^{-1}_{j,j}}}.\]
The interpretation of the above null hypothesis is that including covariate $X_j$ in the model does not significantly improve accuracy of predicting $Y$.  <br><br>

We can also test for significance/insignificance of several covariates at a time using *partial F tests*.  Let $X^R$ denote a subset of the columns of the design matrix $X$ where we have removed columns corresponding to a subset of $r$ of the coefficients in $\beta$.  Fit the model $Y = X^R\beta^R + \epsilon$ and compute the sum of squared residuals for this **reduced** model, call it, $\text{SSE}^R$. Let $\text{SSE}^F$ denote the sum of squared residuals for the **full** model with all $p$ covariates.  Define
\[F = \frac{(SSE^R - SSE^F)/r }{SSE^F / (n-p-1)}.\]
Then, under $H_0:\text{all of these }r\text{ covariates have coefficients equal to zero}$ we have $F\sim F_{r, n-p-1}$, that is, the test statistic $F$ has an $F$ distribution with numerator and denominator degrees of freedom $r$ and $n-p-1$.  We reject the null hypothesis at level $\alpha$ if $F > F_{1-\alpha, r, n-p-1}$.


### Example: Housing price data



```{=html}
<a href="data:text/csv;base64,Tm8sWDEgdHJhbnNhY3Rpb24gZGF0ZSxYMiBob3VzZSBhZ2UsWDMgZGlzdGFuY2UgdG8gdGhlIG5lYXJlc3QgTVJUIHN0YXRpb24sWDQgbnVtYmVyIG9mIGNvbnZlbmllbmNlIHN0b3JlcyxYNSBsYXRpdHVkZSxYNiBsb25naXR1ZGUsWSBob3VzZSBwcmljZSBvZiB1bml0IGFyZWENCjEsMjAxMi45MTcsMzIsODQuODc4ODIsMTAsMjQuOTgyOTgsMTIxLjU0MDI0LDM3LjkNCjIsMjAxMi45MTcsMTkuNSwzMDYuNTk0Nyw5LDI0Ljk4MDM0LDEyMS41Mzk1MSw0Mi4yDQozLDIwMTMuNTgzLDEzLjMsNTYxLjk4NDUsNSwyNC45ODc0NiwxMjEuNTQzOTEsNDcuMw0KNCwyMDEzLjUwMCwxMy4zLDU2MS45ODQ1LDUsMjQuOTg3NDYsMTIxLjU0MzkxLDU0LjgNCjUsMjAxMi44MzMsNSwzOTAuNTY4NCw1LDI0Ljk3OTM3LDEyMS41NDI0NSw0My4xDQo2LDIwMTIuNjY3LDcuMSwyMTc1LjAzLDMsMjQuOTYzMDUsMTIxLjUxMjU0LDMyLjENCjcsMjAxMi42NjcsMzQuNSw2MjMuNDczMSw3LDI0Ljk3OTMzLDEyMS41MzY0Miw0MC4zDQo4LDIwMTMuNDE3LDIwLjMsMjg3LjYwMjUsNiwyNC45ODA0MiwxMjEuNTQyMjgsNDYuNw0KOSwyMDEzLjUwMCwzMS43LDU1MTIuMDM4LDEsMjQuOTUwOTUsMTIxLjQ4NDU4LDE4LjgNCjEwLDIwMTMuNDE3LDE3LjksMTc4My4xOCwzLDI0Ljk2NzMxLDEyMS41MTQ4NiwyMi4xDQoxMSwyMDEzLjA4MywzNC44LDQwNS4yMTM0LDEsMjQuOTczNDksMTIxLjUzMzcyLDQxLjQNCjEyLDIwMTMuMzMzLDYuMyw5MC40NTYwNiw5LDI0Ljk3NDMzLDEyMS41NDMxLDU4LjENCjEzLDIwMTIuOTE3LDEzLDQ5Mi4yMzEzLDUsMjQuOTY1MTUsMTIxLjUzNzM3LDM5LjMNCjE0LDIwMTIuNjY3LDIwLjQsMjQ2OS42NDUsNCwyNC45NjEwOCwxMjEuNTEwNDYsMjMuOA0KMTUsMjAxMy41MDAsMTMuMiwxMTY0LjgzOCw0LDI0Ljk5MTU2LDEyMS41MzQwNiwzNC4zDQoxNiwyMDEzLjU4MywzNS43LDU3OS4yMDgzLDIsMjQuOTgyNCwxMjEuNTQ2MTksNTAuNQ0KMTcsMjAxMy4yNTAsMCwyOTIuOTk3OCw2LDI0Ljk3NzQ0LDEyMS41NDQ1OCw3MC4xDQoxOCwyMDEyLjc1MCwxNy43LDM1MC44NTE1LDEsMjQuOTc1NDQsMTIxLjUzMTE5LDM3LjQNCjE5LDIwMTMuNDE3LDE2LjksMzY4LjEzNjMsOCwyNC45Njc1LDEyMS41NDQ1MSw0Mi4zDQoyMCwyMDEyLjY2NywxLjUsMjMuMzgyODQsNywyNC45Njc3MiwxMjEuNTQxMDIsNDcuNw0KMjEsMjAxMy40MTcsNC41LDIyNzUuODc3LDMsMjQuOTYzMTQsMTIxLjUxMTUxLDI5LjMNCjIyLDIwMTMuNDE3LDEwLjUsMjc5LjE3MjYsNywyNC45NzUyOCwxMjEuNTQ1NDEsNTEuNg0KMjMsMjAxMi45MTcsMTQuNywxMzYwLjEzOSwxLDI0Ljk1MjA0LDEyMS41NDg0MiwyNC42DQoyNCwyMDEzLjA4MywxMC4xLDI3OS4xNzI2LDcsMjQuOTc1MjgsMTIxLjU0NTQxLDQ3LjkNCjI1LDIwMTMuMDAwLDM5LjYsNDgwLjY5NzcsNCwyNC45NzM1MywxMjEuNTM4ODUsMzguOA0KMjYsMjAxMy4wODMsMjkuMywxNDg3Ljg2OCwyLDI0Ljk3NTQyLDEyMS41MTcyNiwyNw0KMjcsMjAxMi42NjcsMy4xLDM4My44NjI0LDUsMjQuOTgwODUsMTIxLjU0MzkxLDU2LjINCjI4LDIwMTMuMjUwLDEwLjQsMjc2LjQ0OSw1LDI0Ljk1NTkzLDEyMS41MzkxMywzMy42DQoyOSwyMDEzLjUwMCwxOS4yLDU1Ny40NzgsNCwyNC45NzQxOSwxMjEuNTM3OTcsNDcNCjMwLDIwMTMuMDgzLDcuMSw0NTEuMjQzOCw1LDI0Ljk3NTYzLDEyMS41NDY5NCw1Ny4xDQozMSwyMDEzLjUwMCwyNS45LDQ1MTkuNjksMCwyNC45NDgyNiwxMjEuNDk1ODcsMjIuMQ0KMzIsMjAxMi43NTAsMjkuNiw3NjkuNDAzNCw3LDI0Ljk4MjgxLDEyMS41MzQwOCwyNQ0KMzMsMjAxMi43NTAsMzcuOSw0ODguNTcyNywxLDI0Ljk3MzQ5LDEyMS41MzQ1MSwzNC4yDQozNCwyMDEzLjI1MCwxNi41LDMyMy42NTUsNiwyNC45Nzg0MSwxMjEuNTQyODEsNDkuMw0KMzUsMjAxMi43NTAsMTUuNCwyMDUuMzY3LDcsMjQuOTg0MTksMTIxLjU0MjQzLDU1LjENCjM2LDIwMTMuNTAwLDEzLjksNDA3OS40MTgsMCwyNS4wMTQ1OSwxMjEuNTE4MTYsMjcuMw0KMzcsMjAxMi45MTcsMTQuNywxOTM1LjAwOSwyLDI0Ljk2Mzg2LDEyMS41MTQ1OCwyMi45DQozOCwyMDEzLjE2NywxMiwxMzYwLjEzOSwxLDI0Ljk1MjA0LDEyMS41NDg0MiwyNS4zDQozOSwyMDEyLjY2NywzLjEsNTc3Ljk2MTUsNiwyNC45NzIwMSwxMjEuNTQ3MjIsNDcuNw0KNDAsMjAxMy4xNjcsMTYuMiwyODkuMzI0OCw1LDI0Ljk4MjAzLDEyMS41NDM0OCw0Ni4yDQo0MSwyMDEzLjAwMCwxMy42LDQwODIuMDE1LDAsMjQuOTQxNTUsMTIxLjUwMzgxLDE1LjkNCjQyLDIwMTMuNTAwLDE2LjgsNDA2Ni41ODcsMCwyNC45NDI5NywxMjEuNTAzNDIsMTguMg0KNDMsMjAxMy40MTcsMzYuMSw1MTkuNDYxNyw1LDI0Ljk2MzA1LDEyMS41Mzc1OCwzNC43DQo0NCwyMDEyLjc1MCwzNC40LDUxMi43ODcxLDYsMjQuOTg3NDgsMTIxLjU0MzAxLDM0LjENCjQ1LDIwMTMuNTgzLDIuNyw1MzMuNDc2Miw0LDI0Ljk3NDQ1LDEyMS41NDc2NSw1My45DQo0NiwyMDEzLjA4MywzNi42LDQ4OC44MTkzLDgsMjQuOTcwMTUsMTIxLjU0NDk0LDM4LjMNCjQ3LDIwMTMuNDE3LDIxLjcsNDYzLjk2MjMsOSwyNC45NzAzLDEyMS41NDQ1OCw0Mg0KNDgsMjAxMy41ODMsMzUuOSw2NDAuNzM5MSwzLDI0Ljk3NTYzLDEyMS41MzcxNSw2MS41DQo0OSwyMDEzLjQxNywyNC4yLDQ2MDUuNzQ5LDAsMjQuOTQ2ODQsMTIxLjQ5NTc4LDEzLjQNCjUwLDIwMTIuNjY3LDI5LjQsNDUxMC4zNTksMSwyNC45NDkyNSwxMjEuNDk1NDIsMTMuMg0KNTEsMjAxMy40MTcsMjEuNyw1MTIuNTQ4Nyw0LDI0Ljk3NCwxMjEuNTM4NDIsNDQuMg0KNTIsMjAxMy4wODMsMzEuMywxNzU4LjQwNiwxLDI0Ljk1NDAyLDEyMS41NTI4MiwyMC43DQo1MywyMDEzLjU4MywzMi4xLDE0MzguNTc5LDMsMjQuOTc0MTksMTIxLjUxNzUsMjcNCjU0LDIwMTMuMDgzLDEzLjMsNDkyLjIzMTMsNSwyNC45NjUxNSwxMjEuNTM3MzcsMzguOQ0KNTUsMjAxMy4wODMsMTYuMSwyODkuMzI0OCw1LDI0Ljk4MjAzLDEyMS41NDM0OCw1MS43DQo1NiwyMDEyLjgzMywzMS43LDExNjAuNjMyLDAsMjQuOTQ5NjgsMTIxLjUzMDA5LDEzLjcNCjU3LDIwMTMuNDE3LDMzLjYsMzcxLjI0OTUsOCwyNC45NzI1NCwxMjEuNTQwNTksNDEuOQ0KNTgsMjAxMi45MTcsMy41LDU2LjQ3NDI1LDcsMjQuOTU3NDQsMTIxLjUzNzExLDUzLjUNCjU5LDIwMTMuNTAwLDMwLjMsNDUxMC4zNTksMSwyNC45NDkyNSwxMjEuNDk1NDIsMjIuNg0KNjAsMjAxMy4wODMsMTMuMywzMzYuMDUzMiw1LDI0Ljk1Nzc2LDEyMS41MzQzOCw0Mi40DQo2MSwyMDEzLjQxNywxMSwxOTMxLjIwNywyLDI0Ljk2MzY1LDEyMS41MTQ3MSwyMS4zDQo2MiwyMDEzLjUwMCw1LjMsMjU5LjY2MDcsNiwyNC45NzU4NSwxMjEuNTQ1MTYsNjMuMg0KNjMsMjAxMi45MTcsMTcuMiwyMTc1Ljg3NywzLDI0Ljk2MzAzLDEyMS41MTI1NCwyNy43DQo2NCwyMDEzLjU4MywyLjYsNTMzLjQ3NjIsNCwyNC45NzQ0NSwxMjEuNTQ3NjUsNTUNCjY1LDIwMTMuMzMzLDE3LjUsOTk1Ljc1NTQsMCwyNC45NjMwNSwxMjEuNTQ5MTUsMjUuMw0KNjYsMjAxMy40MTcsNDAuMSwxMjMuNzQyOSw4LDI0Ljk3NjM1LDEyMS41NDMyOSw0NC4zDQo2NywyMDEzLjAwMCwxLDE5My41ODQ1LDYsMjQuOTY1NzEsMTIxLjU0MDg5LDUwLjcNCjY4LDIwMTMuNTAwLDguNSwxMDQuODEwMSw1LDI0Ljk2Njc0LDEyMS41NDA2Nyw1Ni44DQo2OSwyMDEzLjQxNywzMC40LDQ2NC4yMjMsNiwyNC45Nzk2NCwxMjEuNTM4MDUsMzYuMg0KNzAsMjAxMi44MzMsMTIuNSw1NjEuOTg0NSw1LDI0Ljk4NzQ2LDEyMS41NDM5MSw0Mg0KNzEsMjAxMy41ODMsNi42LDkwLjQ1NjA2LDksMjQuOTc0MzMsMTIxLjU0MzEsNTkNCjcyLDIwMTMuMDgzLDM1LjUsNjQwLjczOTEsMywyNC45NzU2MywxMjEuNTM3MTUsNDAuOA0KNzMsMjAxMy41ODMsMzIuNSw0MjQuNTQ0Miw4LDI0Ljk3NTg3LDEyMS41MzkxMywzNi4zDQo3NCwyMDEzLjE2NywxMy44LDQwODIuMDE1LDAsMjQuOTQxNTUsMTIxLjUwMzgxLDIwDQo3NSwyMDEyLjkxNyw2LjgsMzc5LjU1NzUsMTAsMjQuOTgzNDMsMTIxLjUzNzYyLDU0LjQNCjc2LDIwMTMuNTAwLDEyLjMsMTM2MC4xMzksMSwyNC45NTIwNCwxMjEuNTQ4NDIsMjkuNQ0KNzcsMjAxMy41ODMsMzUuOSw2MTYuNDAwNCwzLDI0Ljk3NzIzLDEyMS41Mzc2NywzNi44DQo3OCwyMDEyLjgzMywyMC41LDIxODUuMTI4LDMsMjQuOTYzMjIsMTIxLjUxMjM3LDI1LjYNCjc5LDIwMTIuOTE3LDM4LjIsNTUyLjQzNzEsMiwyNC45NzU5OCwxMjEuNTMzODEsMjkuOA0KODAsMjAxMy4wMDAsMTgsMTQxNC44MzcsMSwyNC45NTE4MiwxMjEuNTQ4ODcsMjYuNQ0KODEsMjAxMy41MDAsMTEuOCw1MzMuNDc2Miw0LDI0Ljk3NDQ1LDEyMS41NDc2NSw0MC4zDQo4MiwyMDEzLjAwMCwzMC44LDM3Ny43OTU2LDYsMjQuOTY0MjcsMTIxLjUzOTY0LDM2LjgNCjgzLDIwMTMuMDgzLDEzLjIsMTUwLjkzNDcsNywyNC45NjcyNSwxMjEuNTQyNTIsNDguMQ0KODQsMjAxMi45MTcsMjUuMywyNzA3LjM5MiwzLDI0Ljk2MDU2LDEyMS41MDgzMSwxNy43DQo4NSwyMDEzLjA4MywxNS4xLDM4My4yODA1LDcsMjQuOTY3MzUsMTIxLjU0NDY0LDQzLjcNCjg2LDIwMTIuNzUwLDAsMzM4Ljk2NzksOSwyNC45Njg1MywxMjEuNTQ0MTMsNTAuOA0KODcsMjAxMi44MzMsMS44LDE0NTUuNzk4LDEsMjQuOTUxMiwxMjEuNTQ5LDI3DQo4OCwyMDEzLjU4MywxNi45LDQwNjYuNTg3LDAsMjQuOTQyOTcsMTIxLjUwMzQyLDE4LjMNCjg5LDIwMTIuOTE3LDguOSwxNDA2LjQzLDAsMjQuOTg1NzMsMTIxLjUyNzU4LDQ4DQo5MCwyMDEzLjUwMCwyMywzOTQ3Ljk0NSwwLDI0Ljk0NzgzLDEyMS41MDI0MywyNS4zDQo5MSwyMDEyLjgzMywwLDI3NC4wMTQ0LDEsMjQuOTc0OCwxMjEuNTMwNTksNDUuNA0KOTIsMjAxMy4yNTAsOS4xLDE0MDIuMDE2LDAsMjQuOTg1NjksMTIxLjUyNzYsNDMuMg0KOTMsMjAxMi45MTcsMjAuNiwyNDY5LjY0NSw0LDI0Ljk2MTA4LDEyMS41MTA0NiwyMS44DQo5NCwyMDEyLjkxNywzMS45LDExNDYuMzI5LDAsMjQuOTQ5MiwxMjEuNTMwNzYsMTYuMQ0KOTUsMjAxMi45MTcsNDAuOSwxNjcuNTk4OSw1LDI0Ljk2NjMsMTIxLjU0MDI2LDQxDQo5NiwyMDEyLjkxNyw4LDEwNC44MTAxLDUsMjQuOTY2NzQsMTIxLjU0MDY3LDUxLjgNCjk3LDIwMTMuNDE3LDYuNCw5MC40NTYwNiw5LDI0Ljk3NDMzLDEyMS41NDMxLDU5LjUNCjk4LDIwMTMuMDgzLDI4LjQsNjE3LjQ0MjQsMywyNC45Nzc0NiwxMjEuNTMyOTksMzQuNg0KOTksMjAxMy40MTcsMTYuNCwyODkuMzI0OCw1LDI0Ljk4MjAzLDEyMS41NDM0OCw1MQ0KMTAwLDIwMTMuNDE3LDYuNCw5MC40NTYwNiw5LDI0Ljk3NDMzLDEyMS41NDMxLDYyLjINCjEwMSwyMDEzLjUwMCwxNy41LDk2NC43NDk2LDQsMjQuOTg4NzIsMTIxLjUzNDExLDM4LjINCjEwMiwyMDEyLjgzMywxMi43LDE3MC4xMjg5LDEsMjQuOTczNzEsMTIxLjUyOTg0LDMyLjkNCjEwMywyMDEzLjA4MywxLjEsMTkzLjU4NDUsNiwyNC45NjU3MSwxMjEuNTQwODksNTQuNA0KMTA0LDIwMTIuNzUwLDAsMjA4LjM5MDUsNiwyNC45NTYxOCwxMjEuNTM4NDQsNDUuNw0KMTA1LDIwMTIuNjY3LDMyLjcsMzkyLjQ0NTksNiwyNC45NjM5OCwxMjEuNTQyNSwzMC41DQoxMDYsMjAxMi44MzMsMCwyOTIuOTk3OCw2LDI0Ljk3NzQ0LDEyMS41NDQ1OCw3MQ0KMTA3LDIwMTMuMDgzLDE3LjIsMTg5LjUxODEsOCwyNC45NzcwNywxMjEuNTQzMDgsNDcuMQ0KMTA4LDIwMTMuMzMzLDEyLjIsMTM2MC4xMzksMSwyNC45NTIwNCwxMjEuNTQ4NDIsMjYuNg0KMTA5LDIwMTMuNDE3LDMxLjQsNTkyLjUwMDYsMiwyNC45NzI2LDEyMS41MzU2MSwzNC4xDQoxMTAsMjAxMy41ODMsNCwyMTQ3LjM3NiwzLDI0Ljk2Mjk5LDEyMS41MTI4NCwyOC40DQoxMTEsMjAxMy4wODMsOC4xLDEwNC44MTAxLDUsMjQuOTY2NzQsMTIxLjU0MDY3LDUxLjYNCjExMiwyMDEzLjU4MywzMy4zLDE5Ni42MTcyLDcsMjQuOTc3MDEsMTIxLjU0MjI0LDM5LjQNCjExMywyMDEzLjQxNyw5LjksMjEwMi40MjcsMywyNC45NjA0NCwxMjEuNTE0NjIsMjMuMQ0KMTE0LDIwMTMuMzMzLDE0LjgsMzkzLjI2MDYsNiwyNC45NjE3MiwxMjEuNTM4MTIsNy42DQoxMTUsMjAxMi42NjcsMzAuNiwxNDMuODM4Myw4LDI0Ljk4MTU1LDEyMS41NDE0Miw1My4zDQoxMTYsMjAxMy4wODMsMjAuNiw3MzcuOTE2MSwyLDI0Ljk4MDkyLDEyMS41NDczOSw0Ni40DQoxMTcsMjAxMy4wMDAsMzAuOSw2Mzk2LjI4MywxLDI0Ljk0Mzc1LDEyMS40Nzg4MywxMi4yDQoxMTgsMjAxMy4wMDAsMTMuNiw0MTk3LjM0OSwwLDI0LjkzODg1LDEyMS41MDM4MywxMw0KMTE5LDIwMTMuNTAwLDI1LjMsMTU4My43MjIsMywyNC45NjYyMiwxMjEuNTE3MDksMzAuNg0KMTIwLDIwMTMuNTAwLDE2LjYsMjg5LjMyNDgsNSwyNC45ODIwMywxMjEuNTQzNDgsNTkuNg0KMTIxLDIwMTMuMTY3LDEzLjMsNDkyLjIzMTMsNSwyNC45NjUxNSwxMjEuNTM3MzcsMzEuMw0KMTIyLDIwMTMuNTAwLDEzLjYsNDkyLjIzMTMsNSwyNC45NjUxNSwxMjEuNTM3MzcsNDgNCjEyMywyMDEzLjI1MCwzMS41LDQxNC45NDc2LDQsMjQuOTgxOTksMTIxLjU0NDY0LDMyLjUNCjEyNCwyMDEzLjQxNywwLDE4NS40Mjk2LDAsMjQuOTcxMSwxMjEuNTMxNyw0NS41DQoxMjUsMjAxMi45MTcsOS45LDI3OS4xNzI2LDcsMjQuOTc1MjgsMTIxLjU0NTQxLDU3LjQNCjEyNiwyMDEzLjE2NywxLjEsMTkzLjU4NDUsNiwyNC45NjU3MSwxMjEuNTQwODksNDguNg0KMTI3LDIwMTMuMDgzLDM4LjYsODA0LjY4OTcsNCwyNC45NzgzOCwxMjEuNTM0NzcsNjIuOQ0KMTI4LDIwMTMuMjUwLDMuOCwzODMuODYyNCw1LDI0Ljk4MDg1LDEyMS41NDM5MSw1NQ0KMTI5LDIwMTMuMDgzLDQxLjMsMTI0Ljk5MTIsNiwyNC45NjY3NCwxMjEuNTQwMzksNjAuNw0KMTMwLDIwMTMuNDE3LDM4LjUsMjE2LjgzMjksNywyNC45ODA4NiwxMjEuNTQxNjIsNDENCjEzMSwyMDEzLjI1MCwyOS42LDUzNS41MjcsOCwyNC45ODA5MiwxMjEuNTM2NTMsMzcuNQ0KMTMyLDIwMTMuNTAwLDQsMjE0Ny4zNzYsMywyNC45NjI5OSwxMjEuNTEyODQsMzAuNw0KMTMzLDIwMTMuMTY3LDI2LjYsNDgyLjc1ODEsNSwyNC45NzQzMywxMjEuNTM4NjMsMzcuNQ0KMTM0LDIwMTIuODMzLDE4LDM3My4zOTM3LDgsMjQuOTg2NiwxMjEuNTQwODIsMzkuNQ0KMTM1LDIwMTIuNjY3LDMzLjQsMTg2Ljk2ODYsNiwyNC45NjYwNCwxMjEuNTQyMTEsNDIuMg0KMTM2LDIwMTIuOTE3LDE4LjksMTAwOS4yMzUsMCwyNC45NjM1NywxMjEuNTQ5NTEsMjAuOA0KMTM3LDIwMTIuNzUwLDExLjQsMzkwLjU2ODQsNSwyNC45NzkzNywxMjEuNTQyNDUsNDYuOA0KMTM4LDIwMTMuNTAwLDEzLjYsMzE5LjA3MDgsNiwyNC45NjQ5NSwxMjEuNTQyNzcsNDcuNA0KMTM5LDIwMTMuMTY3LDEwLDk0Mi40NjY0LDAsMjQuOTc4NDMsMTIxLjUyNDA2LDQzLjUNCjE0MCwyMDEyLjY2NywxMi45LDQ5Mi4yMzEzLDUsMjQuOTY1MTUsMTIxLjUzNzM3LDQyLjUNCjE0MSwyMDEzLjI1MCwxNi4yLDI4OS4zMjQ4LDUsMjQuOTgyMDMsMTIxLjU0MzQ4LDUxLjQNCjE0MiwyMDEzLjMzMyw1LjEsMTU1OS44MjcsMywyNC45NzIxMywxMjEuNTE2MjcsMjguOQ0KMTQzLDIwMTMuNDE3LDE5LjgsNjQwLjYwNzEsNSwyNC45NzAxNywxMjEuNTQ2NDcsMzcuNQ0KMTQ0LDIwMTMuNTAwLDEzLjYsNDkyLjIzMTMsNSwyNC45NjUxNSwxMjEuNTM3MzcsNDAuMQ0KMTQ1LDIwMTMuMDgzLDExLjksMTM2MC4xMzksMSwyNC45NTIwNCwxMjEuNTQ4NDIsMjguNA0KMTQ2LDIwMTIuOTE3LDIuMSw0NTEuMjQzOCw1LDI0Ljk3NTYzLDEyMS41NDY5NCw0NS41DQoxNDcsMjAxMi43NTAsMCwxODUuNDI5NiwwLDI0Ljk3MTEsMTIxLjUzMTcsNTIuMg0KMTQ4LDIwMTIuNzUwLDMuMiw0ODkuODgyMSw4LDI0Ljk3MDE3LDEyMS41NDQ5NCw0My4yDQoxNDksMjAxMy41MDAsMTYuNCwzNzgwLjU5LDAsMjQuOTMyOTMsMTIxLjUxMjAzLDQ1LjENCjE1MCwyMDEyLjY2NywzNC45LDE3OS40NTM4LDgsMjQuOTczNDksMTIxLjU0MjQ1LDM5LjcNCjE1MSwyMDEzLjI1MCwzNS44LDE3MC43MzExLDcsMjQuOTY3MTksMTIxLjU0MjY5LDQ4LjUNCjE1MiwyMDEzLjUwMCw0LjksMzg3Ljc3MjEsOSwyNC45ODExOCwxMjEuNTM3ODgsNDQuNw0KMTUzLDIwMTMuMzMzLDEyLDEzNjAuMTM5LDEsMjQuOTUyMDQsMTIxLjU0ODQyLDI4LjkNCjE1NCwyMDEzLjI1MCw2LjUsMzc2LjE3MDksNiwyNC45NTQxOCwxMjEuNTM3MTMsNDAuOQ0KMTU1LDIwMTMuNTAwLDE2LjksNDA2Ni41ODcsMCwyNC45NDI5NywxMjEuNTAzNDIsMjAuNw0KMTU2LDIwMTMuMTY3LDEzLjgsNDA4Mi4wMTUsMCwyNC45NDE1NSwxMjEuNTAzODEsMTUuNg0KMTU3LDIwMTMuNTgzLDMwLjcsMTI2NC43MywwLDI0Ljk0ODgzLDEyMS41Mjk1NCwxOC4zDQoxNTgsMjAxMy4yNTAsMTYuMSw4MTUuOTMxNCw0LDI0Ljk3ODg2LDEyMS41MzQ2NCwzNS42DQoxNTksMjAxMy4wMDAsMTEuNiwzOTAuNTY4NCw1LDI0Ljk3OTM3LDEyMS41NDI0NSwzOS40DQoxNjAsMjAxMi42NjcsMTUuNSw4MTUuOTMxNCw0LDI0Ljk3ODg2LDEyMS41MzQ2NCwzNy40DQoxNjEsMjAxMi45MTcsMy41LDQ5LjY2MTA1LDgsMjQuOTU4MzYsMTIxLjUzNzU2LDU3LjgNCjE2MiwyMDEzLjQxNywxOS4yLDYxNi40MDA0LDMsMjQuOTc3MjMsMTIxLjUzNzY3LDM5LjYNCjE2MywyMDEyLjc1MCwxNiw0MDY2LjU4NywwLDI0Ljk0Mjk3LDEyMS41MDM0MiwxMS42DQoxNjQsMjAxMy41MDAsOC41LDEwNC44MTAxLDUsMjQuOTY2NzQsMTIxLjU0MDY3LDU1LjUNCjE2NSwyMDEyLjgzMywwLDE4NS40Mjk2LDAsMjQuOTcxMSwxMjEuNTMxNyw1NS4yDQoxNjYsMjAxMi45MTcsMTMuNywxMjM2LjU2NCwxLDI0Ljk3Njk0LDEyMS41NTM5MSwzMC42DQoxNjcsMjAxMy40MTcsMCwyOTIuOTk3OCw2LDI0Ljk3NzQ0LDEyMS41NDQ1OCw3My42DQoxNjgsMjAxMy40MTcsMjguMiwzMzAuMDg1NCw4LDI0Ljk3NDA4LDEyMS41NDAxMSw0My40DQoxNjksMjAxMy4wODMsMjcuNiw1MTUuMTEyMiw1LDI0Ljk2Mjk5LDEyMS41NDMyLDM3LjQNCjE3MCwyMDEzLjQxNyw4LjQsMTk2Mi42MjgsMSwyNC45NTQ2OCwxMjEuNTU0ODEsMjMuNQ0KMTcxLDIwMTMuMzMzLDI0LDQ1MjcuNjg3LDAsMjQuOTQ3NDEsMTIxLjQ5NjI4LDE0LjQNCjE3MiwyMDEzLjA4MywzLjYsMzgzLjg2MjQsNSwyNC45ODA4NSwxMjEuNTQzOTEsNTguOA0KMTczLDIwMTMuNTgzLDYuNiw5MC40NTYwNiw5LDI0Ljk3NDMzLDEyMS41NDMxLDU4LjENCjE3NCwyMDEzLjA4Myw0MS4zLDQwMS44ODA3LDQsMjQuOTgzMjYsMTIxLjU0NDYsMzUuMQ0KMTc1LDIwMTMuNDE3LDQuMyw0MzIuMDM4NSw3LDI0Ljk4MDUsMTIxLjUzNzc4LDQ1LjINCjE3NiwyMDEzLjA4MywzMC4yLDQ3Mi4xNzQ1LDMsMjQuOTcwMDUsMTIxLjUzNzU4LDM2LjUNCjE3NywyMDEyLjgzMywxMy45LDQ1NzMuNzc5LDAsMjQuOTQ4NjcsMTIxLjQ5NTA3LDE5LjINCjE3OCwyMDEzLjA4MywzMywxODEuMDc2Niw5LDI0Ljk3Njk3LDEyMS41NDI2Miw0Mg0KMTc5LDIwMTMuNTAwLDEzLjEsMTE0NC40MzYsNCwyNC45OTE3NiwxMjEuNTM0NTYsMzYuNw0KMTgwLDIwMTMuMDgzLDE0LDQzOC44NTEzLDEsMjQuOTc0OTMsMTIxLjUyNzMsNDIuNg0KMTgxLDIwMTIuNjY3LDI2LjksNDQ0OS4yNywwLDI0Ljk0ODk4LDEyMS40OTYyMSwxNS41DQoxODIsMjAxMy4xNjcsMTEuNiwyMDEuODkzOSw4LDI0Ljk4NDg5LDEyMS41NDEyMSw1NS45DQoxODMsMjAxMy41MDAsMTMuNSwyMTQ3LjM3NiwzLDI0Ljk2Mjk5LDEyMS41MTI4NCwyMy42DQoxODQsMjAxMy41MDAsMTcsNDA4Mi4wMTUsMCwyNC45NDE1NSwxMjEuNTAzODEsMTguOA0KMTg1LDIwMTIuNzUwLDE0LjEsMjYxNS40NjUsMCwyNC45NTQ5NSwxMjEuNTYxNzQsMjEuOA0KMTg2LDIwMTIuNzUwLDMxLjQsMTQ0Ny4yODYsMywyNC45NzI4NSwxMjEuNTE3MywyMS41DQoxODcsMjAxMy4xNjcsMjAuOSwyMTg1LjEyOCwzLDI0Ljk2MzIyLDEyMS41MTIzNywyNS43DQoxODgsMjAxMy4wMDAsOC45LDMwNzguMTc2LDAsMjQuOTU0NjQsMTIxLjU2NjI3LDIyDQoxODksMjAxMi45MTcsMzQuOCwxOTAuMDM5Miw4LDI0Ljk3NzA3LDEyMS41NDMxMiw0NC4zDQoxOTAsMjAxMi45MTcsMTYuMyw0MDY2LjU4NywwLDI0Ljk0Mjk3LDEyMS41MDM0MiwyMC41DQoxOTEsMjAxMy41MDAsMzUuMyw2MTYuNTczNSw4LDI0Ljk3OTQ1LDEyMS41MzY0Miw0Mi4zDQoxOTIsMjAxMy4xNjcsMTMuMiw3NTAuMDcwNCwyLDI0Ljk3MzcxLDEyMS41NDk1MSwzNy44DQoxOTMsMjAxMy4xNjcsNDMuOCw1Ny41ODk0NSw3LDI0Ljk2NzUsMTIxLjU0MDY5LDQyLjcNCjE5NCwyMDEzLjQxNyw5LjcsNDIxLjQ3OSw1LDI0Ljk4MjQ2LDEyMS41NDQ3Nyw0OS4zDQoxOTUsMjAxMy41MDAsMTUuMiwzNzcxLjg5NSwwLDI0LjkzMzYzLDEyMS41MTE1OCwyOS4zDQoxOTYsMjAxMy4zMzMsMTUuMiw0NjEuMTAxNiw1LDI0Ljk1NDI1LDEyMS41Mzk5LDM0LjYNCjE5NywyMDEzLjAwMCwyMi44LDcwNy45MDY3LDIsMjQuOTgxLDEyMS41NDcxMywzNi42DQoxOTgsMjAxMy4yNTAsMzQuNCwxMjYuNzI4Niw4LDI0Ljk2ODgxLDEyMS41NDA4OSw0OC4yDQoxOTksMjAxMy4wODMsMzQsMTU3LjYwNTIsNywyNC45NjYyOCwxMjEuNTQxOTYsMzkuMQ0KMjAwLDIwMTMuNDE3LDE4LjIsNDUxLjY0MTksOCwyNC45Njk0NSwxMjEuNTQ0OSwzMS42DQoyMDEsMjAxMy40MTcsMTcuNCw5OTUuNzU1NCwwLDI0Ljk2MzA1LDEyMS41NDkxNSwyNS41DQoyMDIsMjAxMy40MTcsMTMuMSw1NjEuOTg0NSw1LDI0Ljk4NzQ2LDEyMS41NDM5MSw0NS45DQoyMDMsMjAxMi45MTcsMzguMyw2NDIuNjk4NSwzLDI0Ljk3NTU5LDEyMS41MzcxMywzMS41DQoyMDQsMjAxMi42NjcsMTUuNiwyODkuMzI0OCw1LDI0Ljk4MjAzLDEyMS41NDM0OCw0Ni4xDQoyMDUsMjAxMy4wMDAsMTgsMTQxNC44MzcsMSwyNC45NTE4MiwxMjEuNTQ4ODcsMjYuNg0KMjA2LDIwMTMuMDgzLDEyLjgsMTQ0OS43MjIsMywyNC45NzI4OSwxMjEuNTE3MjgsMjEuNA0KMjA3LDIwMTMuMjUwLDIyLjIsMzc5LjU1NzUsMTAsMjQuOTgzNDMsMTIxLjUzNzYyLDQ0DQoyMDgsMjAxMy4wODMsMzguNSw2NjUuMDYzNiwzLDI0Ljk3NTAzLDEyMS41MzY5MiwzNC4yDQoyMDksMjAxMi43NTAsMTEuNSwxMzYwLjEzOSwxLDI0Ljk1MjA0LDEyMS41NDg0MiwyNi4yDQoyMTAsMjAxMi44MzMsMzQuOCwxNzUuNjI5NCw4LDI0Ljk3MzQ3LDEyMS41NDI3MSw0MC45DQoyMTEsMjAxMy41MDAsNS4yLDM5MC41Njg0LDUsMjQuOTc5MzcsMTIxLjU0MjQ1LDUyLjINCjIxMiwyMDEzLjA4MywwLDI3NC4wMTQ0LDEsMjQuOTc0OCwxMjEuNTMwNTksNDMuNQ0KMjEzLDIwMTMuMzMzLDE3LjYsMTgwNS42NjUsMiwyNC45ODY3MiwxMjEuNTIwOTEsMzEuMQ0KMjE0LDIwMTMuMDgzLDYuMiw5MC40NTYwNiw5LDI0Ljk3NDMzLDEyMS41NDMxLDU4DQoyMTUsMjAxMy41ODMsMTguMSwxNzgzLjE4LDMsMjQuOTY3MzEsMTIxLjUxNDg2LDIwLjkNCjIxNiwyMDEzLjMzMywxOS4yLDM4My43MTI5LDgsMjQuOTcyLDEyMS41NDQ3Nyw0OC4xDQoyMTcsMjAxMy4yNTAsMzcuOCw1OTAuOTI5MiwxLDI0Ljk3MTUzLDEyMS41MzU1OSwzOS43DQoyMTgsMjAxMi45MTcsMjgsMzcyLjYyNDIsNiwyNC45NzgzOCwxMjEuNTQxMTksNDAuOA0KMjE5LDIwMTMuNDE3LDEzLjYsNDkyLjIzMTMsNSwyNC45NjUxNSwxMjEuNTM3MzcsNDMuOA0KMjIwLDIwMTIuNzUwLDI5LjMsNTI5Ljc3NzEsOCwyNC45ODEwMiwxMjEuNTM2NTUsNDAuMg0KMjIxLDIwMTMuMzMzLDM3LjIsMTg2LjUxMDEsOSwyNC45NzcwMywxMjEuNTQyNjUsNzguMw0KMjIyLDIwMTMuMzMzLDksMTQwMi4wMTYsMCwyNC45ODU2OSwxMjEuNTI3NiwzOC41DQoyMjMsMjAxMy41ODMsMzAuNiw0MzEuMTExNCwxMCwyNC45ODEyMywxMjEuNTM3NDMsNDguNQ0KMjI0LDIwMTMuMjUwLDkuMSwxNDAyLjAxNiwwLDI0Ljk4NTY5LDEyMS41Mjc2LDQyLjMNCjIyNSwyMDEzLjMzMywzNC41LDMyNC45NDE5LDYsMjQuOTc4MTQsMTIxLjU0MTcsNDYNCjIyNiwyMDEzLjI1MCwxLjEsMTkzLjU4NDUsNiwyNC45NjU3MSwxMjEuNTQwODksNDkNCjIyNywyMDEzLjAwMCwxNi41LDQwODIuMDE1LDAsMjQuOTQxNTUsMTIxLjUwMzgxLDEyLjgNCjIyOCwyMDEyLjkxNywzMi40LDI2NS4wNjA5LDgsMjQuOTgwNTksMTIxLjUzOTg2LDQwLjINCjIyOSwyMDEzLjQxNywxMS45LDMxNzEuMzI5LDAsMjUuMDAxMTUsMTIxLjUxNzc2LDQ2LjYNCjIzMCwyMDEzLjU4MywzMSwxMTU2LjQxMiwwLDI0Ljk0ODksMTIxLjUzMDk1LDE5DQoyMzEsMjAxMy41MDAsNCwyMTQ3LjM3NiwzLDI0Ljk2Mjk5LDEyMS41MTI4NCwzMy40DQoyMzIsMjAxMi44MzMsMTYuMiw0MDc0LjczNiwwLDI0Ljk0MjM1LDEyMS41MDM1NywxNC43DQoyMzMsMjAxMi45MTcsMjcuMSw0NDEyLjc2NSwxLDI0Ljk1MDMyLDEyMS40OTU4NywxNy40DQoyMzQsMjAxMy4zMzMsMzkuNywzMzMuMzY3OSw5LDI0Ljk4MDE2LDEyMS41MzkzMiwzMi40DQoyMzUsMjAxMy4yNTAsOCwyMjE2LjYxMiw0LDI0Ljk2MDA3LDEyMS41MTM2MSwyMy45DQoyMzYsMjAxMi43NTAsMTIuOSwyNTAuNjMxLDcsMjQuOTY2MDYsMTIxLjU0Mjk3LDM5LjMNCjIzNywyMDEzLjE2NywzLjYsMzczLjgzODksMTAsMjQuOTgzMjIsMTIxLjUzNzY1LDYxLjkNCjIzOCwyMDEzLjE2NywxMyw3MzIuODUyOCwwLDI0Ljk3NjY4LDEyMS41MjUxOCwzOQ0KMjM5LDIwMTMuMDgzLDEyLjgsNzMyLjg1MjgsMCwyNC45NzY2OCwxMjEuNTI1MTgsNDAuNg0KMjQwLDIwMTMuNTAwLDE4LjEsODM3LjcyMzMsMCwyNC45NjMzNCwxMjEuNTQ3NjcsMjkuNw0KMjQxLDIwMTMuMDgzLDExLDE3MTIuNjMyLDIsMjQuOTY0MTIsMTIxLjUxNjcsMjguOA0KMjQyLDIwMTMuNTAwLDEzLjcsMjUwLjYzMSw3LDI0Ljk2NjA2LDEyMS41NDI5Nyw0MS40DQoyNDMsMjAxMi44MzMsMiwyMDc3LjM5LDMsMjQuOTYzNTcsMTIxLjUxMzI5LDMzLjQNCjI0NCwyMDEzLjQxNywzMi44LDIwNC4xNzA1LDgsMjQuOTgyMzYsMTIxLjUzOTIzLDQ4LjINCjI0NSwyMDEzLjA4Myw0LjgsMTU1OS44MjcsMywyNC45NzIxMywxMjEuNTE2MjcsMjEuNw0KMjQ2LDIwMTMuNDE3LDcuNSw2MzkuNjE5OCw1LDI0Ljk3MjU4LDEyMS41NDgxNCw0MC44DQoyNDcsMjAxMy40MTcsMTYuNCwzODkuODIxOSw2LDI0Ljk2NDEyLDEyMS41NDI3Myw0MC42DQoyNDgsMjAxMy4zMzMsMjEuNywxMDU1LjA2NywwLDI0Ljk2MjExLDEyMS41NDkyOCwyMy4xDQoyNDksMjAxMy4wMDAsMTksMTAwOS4yMzUsMCwyNC45NjM1NywxMjEuNTQ5NTEsMjIuMw0KMjUwLDIwMTIuODMzLDE4LDYzMDYuMTUzLDEsMjQuOTU3NDMsMTIxLjQ3NTE2LDE1DQoyNTEsMjAxMy4xNjcsMzkuMiw0MjQuNzEzMiw3LDI0Ljk3NDI5LDEyMS41MzkxNywzMA0KMjUyLDIwMTIuOTE3LDMxLjcsMTE1OS40NTQsMCwyNC45NDk2LDEyMS41MzAxOCwxMy44DQoyNTMsMjAxMi44MzMsNS45LDkwLjQ1NjA2LDksMjQuOTc0MzMsMTIxLjU0MzEsNTIuNw0KMjU0LDIwMTIuNjY3LDMwLjQsMTczNS41OTUsMiwyNC45NjQ2NCwxMjEuNTE2MjMsMjUuOQ0KMjU1LDIwMTIuNjY3LDEuMSwzMjkuOTc0Nyw1LDI0Ljk4MjU0LDEyMS41NDM5NSw1MS44DQoyNTYsMjAxMy40MTcsMzEuNSw1NTEyLjAzOCwxLDI0Ljk1MDk1LDEyMS40ODQ1OCwxNy40DQoyNTcsMjAxMi42NjcsMTQuNiwzMzkuMjI4OSwxLDI0Ljk3NTE5LDEyMS41MzE1MSwyNi41DQoyNTgsMjAxMy4yNTAsMTcuMyw0NDQuMTMzNCwxLDI0Ljk3NTAxLDEyMS41MjczLDQzLjkNCjI1OSwyMDEzLjQxNywwLDI5Mi45OTc4LDYsMjQuOTc3NDQsMTIxLjU0NDU4LDYzLjMNCjI2MCwyMDEzLjA4MywxNy43LDgzNy43MjMzLDAsMjQuOTYzMzQsMTIxLjU0NzY3LDI4LjgNCjI2MSwyMDEzLjI1MCwxNywxNDg1LjA5Nyw0LDI0Ljk3MDczLDEyMS41MTcsMzAuNw0KMjYyLDIwMTMuMTY3LDE2LjIsMjI4OC4wMTEsMywyNC45NTg4NSwxMjEuNTEzNTksMjQuNA0KMjYzLDIwMTIuOTE3LDE1LjksMjg5LjMyNDgsNSwyNC45ODIwMywxMjEuNTQzNDgsNTMNCjI2NCwyMDEzLjQxNywzLjksMjE0Ny4zNzYsMywyNC45NjI5OSwxMjEuNTEyODQsMzEuNw0KMjY1LDIwMTMuMTY3LDMyLjYsNDkzLjY1Nyw3LDI0Ljk2OTY4LDEyMS41NDUyMiw0MC42DQoyNjYsMjAxMi44MzMsMTUuNyw4MTUuOTMxNCw0LDI0Ljk3ODg2LDEyMS41MzQ2NCwzOC4xDQoyNjcsMjAxMy4yNTAsMTcuOCwxNzgzLjE4LDMsMjQuOTY3MzEsMTIxLjUxNDg2LDIzLjcNCjI2OCwyMDEyLjgzMywzNC43LDQ4Mi43NTgxLDUsMjQuOTc0MzMsMTIxLjUzODYzLDQxLjENCjI2OSwyMDEzLjQxNywxNy4yLDM5MC41Njg0LDUsMjQuOTc5MzcsMTIxLjU0MjQ1LDQwLjENCjI3MCwyMDEzLjAwMCwxNy42LDgzNy43MjMzLDAsMjQuOTYzMzQsMTIxLjU0NzY3LDIzDQoyNzEsMjAxMy4zMzMsMTAuOCwyNTIuNTgyMiwxLDI0Ljk3NDYsMTIxLjUzMDQ2LDExNy41DQoyNzIsMjAxMi45MTcsMTcuNyw0NTEuNjQxOSw4LDI0Ljk2OTQ1LDEyMS41NDQ5LDI2LjUNCjI3MywyMDEyLjc1MCwxMyw0OTIuMjMxMyw1LDI0Ljk2NTE1LDEyMS41MzczNyw0MC41DQoyNzQsMjAxMy40MTcsMTMuMiwxNzAuMTI4OSwxLDI0Ljk3MzcxLDEyMS41Mjk4NCwyOS4zDQoyNzUsMjAxMy4xNjcsMjcuNSwzOTQuMDE3Myw3LDI0Ljk3MzA1LDEyMS41Mzk5NCw0MQ0KMjc2LDIwMTIuNjY3LDEuNSwyMy4zODI4NCw3LDI0Ljk2NzcyLDEyMS41NDEwMiw0OS43DQoyNzcsMjAxMy4wMDAsMTkuMSw0NjEuMTAxNiw1LDI0Ljk1NDI1LDEyMS41Mzk5LDM0DQoyNzgsMjAxMy40MTcsMjEuMiwyMTg1LjEyOCwzLDI0Ljk2MzIyLDEyMS41MTIzNywyNy43DQoyNzksMjAxMi43NTAsMCwyMDguMzkwNSw2LDI0Ljk1NjE4LDEyMS41Mzg0NCw0NA0KMjgwLDIwMTMuNDE3LDIuNiwxNTU0LjI1LDMsMjQuOTcwMjYsMTIxLjUxNjQyLDMxLjENCjI4MSwyMDEzLjI1MCwyLjMsMTg0LjMzMDIsNiwyNC45NjU4MSwxMjEuNTQwODYsNDUuNA0KMjgyLDIwMTMuMzMzLDQuNywzODcuNzcyMSw5LDI0Ljk4MTE4LDEyMS41Mzc4OCw0NC44DQoyODMsMjAxMi45MTcsMiwxNDU1Ljc5OCwxLDI0Ljk1MTIsMTIxLjU0OSwyNS42DQoyODQsMjAxMy40MTcsMzMuNSwxOTc4LjY3MSwyLDI0Ljk4Njc0LDEyMS41MTg0NCwyMy41DQoyODUsMjAxMi45MTcsMTUsMzgzLjI4MDUsNywyNC45NjczNSwxMjEuNTQ0NjQsMzQuNA0KMjg2LDIwMTMuMTY3LDMwLjEsNzE4LjI5MzcsMywyNC45NzUwOSwxMjEuNTM2NDQsNTUuMw0KMjg3LDIwMTIuOTE3LDUuOSw5MC40NTYwNiw5LDI0Ljk3NDMzLDEyMS41NDMxLDU2LjMNCjI4OCwyMDEzLjAwMCwxOS4yLDQ2MS4xMDE2LDUsMjQuOTU0MjUsMTIxLjUzOTksMzIuOQ0KMjg5LDIwMTMuNTgzLDE2LjYsMzIzLjY5MTIsNiwyNC45Nzg0MSwxMjEuNTQyOCw1MQ0KMjkwLDIwMTMuMzMzLDEzLjksMjg5LjMyNDgsNSwyNC45ODIwMywxMjEuNTQzNDgsNDQuNQ0KMjkxLDIwMTMuMDgzLDM3LjcsNDkwLjM0NDYsMCwyNC45NzIxNywxMjEuNTM0NzEsMzcNCjI5MiwyMDEyLjgzMywzLjQsNTYuNDc0MjUsNywyNC45NTc0NCwxMjEuNTM3MTEsNTQuNA0KMjkzLDIwMTMuMDgzLDE3LjUsMzk1LjY3NDcsNSwyNC45NTY3NCwxMjEuNTM0LDI0LjUNCjI5NCwyMDEyLjY2NywxMi42LDM4My4yODA1LDcsMjQuOTY3MzUsMTIxLjU0NDY0LDQyLjUNCjI5NSwyMDEzLjUwMCwyNi40LDMzNS41MjczLDYsMjQuOTc5NiwxMjEuNTQxNCwzOC4xDQoyOTYsMjAxMy4xNjcsMTguMiwyMTc5LjU5LDMsMjQuOTYyOTksMTIxLjUxMjUyLDIxLjgNCjI5NywyMDEyLjc1MCwxMi41LDExNDQuNDM2LDQsMjQuOTkxNzYsMTIxLjUzNDU2LDM0LjENCjI5OCwyMDEyLjgzMywzNC45LDU2Ny4wMzQ5LDQsMjQuOTcwMDMsMTIxLjU0NTgsMjguNQ0KMjk5LDIwMTMuMzMzLDE2LjcsNDA4Mi4wMTUsMCwyNC45NDE1NSwxMjEuNTAzODEsMTYuNw0KMzAwLDIwMTMuMTY3LDMzLjIsMTIxLjcyNjIsMTAsMjQuOTgxNzgsMTIxLjU0MDU5LDQ2LjENCjMwMSwyMDEzLjA4MywyLjUsMTU2LjI0NDIsNCwyNC45NjY5NiwxMjEuNTM5OTIsMzYuOQ0KMzAyLDIwMTIuNzUwLDM4LDQ2MS43ODQ4LDAsMjQuOTcyMjksMTIxLjUzNDQ1LDM1LjcNCjMwMywyMDEzLjUwMCwxNi41LDIyODguMDExLDMsMjQuOTU4ODUsMTIxLjUxMzU5LDIzLjINCjMwNCwyMDEzLjUwMCwzOC4zLDQzOS43MTA1LDAsMjQuOTcxNjEsMTIxLjUzNDIzLDM4LjQNCjMwNSwyMDEzLjQxNywyMCwxNjI2LjA4MywzLDI0Ljk2NjIyLDEyMS41MTY2OCwyOS40DQozMDYsMjAxMy4wODMsMTYuMiwyODkuMzI0OCw1LDI0Ljk4MjAzLDEyMS41NDM0OCw1NQ0KMzA3LDIwMTMuNTAwLDE0LjQsMTY5Ljk4MDMsMSwyNC45NzM2OSwxMjEuNTI5NzksNTAuMg0KMzA4LDIwMTIuODMzLDEwLjMsMzA3OS44OSwwLDI0Ljk1NDYsMTIxLjU2NjI3LDI0LjcNCjMwOSwyMDEzLjQxNywxNi40LDI4OS4zMjQ4LDUsMjQuOTgyMDMsMTIxLjU0MzQ4LDUzDQozMTAsMjAxMy4yNTAsMzAuMywxMjY0LjczLDAsMjQuOTQ4ODMsMTIxLjUyOTU0LDE5LjENCjMxMSwyMDEzLjU4MywxNi40LDE2NDMuNDk5LDIsMjQuOTUzOTQsMTIxLjU1MTc0LDI0LjcNCjMxMiwyMDEzLjE2NywyMS4zLDUzNy43OTcxLDQsMjQuOTc0MjUsMTIxLjUzODE0LDQyLjINCjMxMywyMDEzLjU4MywzNS40LDMxOC41MjkyLDksMjQuOTcwNzEsMTIxLjU0MDY5LDc4DQozMTQsMjAxMy4zMzMsOC4zLDEwNC44MTAxLDUsMjQuOTY2NzQsMTIxLjU0MDY3LDQyLjgNCjMxNSwyMDEzLjI1MCwzLjcsNTc3Ljk2MTUsNiwyNC45NzIwMSwxMjEuNTQ3MjIsNDEuNg0KMzE2LDIwMTMuMDgzLDE1LjYsMTc1Ni40MTEsMiwyNC45ODMyLDEyMS41MTgxMiwyNy4zDQozMTcsMjAxMy4yNTAsMTMuMywyNTAuNjMxLDcsMjQuOTY2MDYsMTIxLjU0Mjk3LDQyDQozMTgsMjAxMi43NTAsMTUuNiw3NTIuNzY2OSwyLDI0Ljk3Nzk1LDEyMS41MzQ1MSwzNy41DQozMTksMjAxMy4zMzMsNy4xLDM3OS41NTc1LDEwLDI0Ljk4MzQzLDEyMS41Mzc2Miw0OS44DQozMjAsMjAxMy4yNTAsMzQuNiwyNzIuNjc4Myw1LDI0Ljk1NTYyLDEyMS41Mzg3MiwyNi45DQozMjEsMjAxMi43NTAsMTMuNSw0MTk3LjM0OSwwLDI0LjkzODg1LDEyMS41MDM4MywxOC42DQozMjIsMjAxMi45MTcsMTYuOSw5NjQuNzQ5Niw0LDI0Ljk4ODcyLDEyMS41MzQxMSwzNy43DQozMjMsMjAxMy4wMDAsMTIuOSwxODcuNDgyMywxLDI0Ljk3Mzg4LDEyMS41Mjk4MSwzMy4xDQozMjQsMjAxMy40MTcsMjguNiwxOTcuMTMzOCw2LDI0Ljk3NjMxLDEyMS41NDQzNiw0Mi41DQozMjUsMjAxMi42NjcsMTIuNCwxNzEyLjYzMiwyLDI0Ljk2NDEyLDEyMS41MTY3LDMxLjMNCjMyNiwyMDEzLjA4MywzNi42LDQ4OC44MTkzLDgsMjQuOTcwMTUsMTIxLjU0NDk0LDM4LjENCjMyNywyMDEzLjUwMCw0LjEsNTYuNDc0MjUsNywyNC45NTc0NCwxMjEuNTM3MTEsNjIuMQ0KMzI4LDIwMTMuNDE3LDMuNSw3NTcuMzM3NywzLDI0Ljk3NTM4LDEyMS41NDk3MSwzNi43DQozMjksMjAxMi44MzMsMTUuOSwxNDk3LjcxMywzLDI0Ljk3MDAzLDEyMS41MTY5NiwyMy42DQozMzAsMjAxMy4wMDAsMTMuNiw0MTk3LjM0OSwwLDI0LjkzODg1LDEyMS41MDM4MywxOS4yDQozMzEsMjAxMy4wODMsMzIsMTE1Ni43NzcsMCwyNC45NDkzNSwxMjEuNTMwNDYsMTIuOA0KMzMyLDIwMTMuMzMzLDI1LjYsNDUxOS42OSwwLDI0Ljk0ODI2LDEyMS40OTU4NywxNS42DQozMzMsMjAxMy4xNjcsMzkuOCw2MTcuNzEzNCwyLDI0Ljk3NTc3LDEyMS41MzQ3NSwzOS42DQozMzQsMjAxMi43NTAsNy44LDEwNC44MTAxLDUsMjQuOTY2NzQsMTIxLjU0MDY3LDM4LjQNCjMzNSwyMDEyLjkxNywzMCwxMDEzLjM0MSw1LDI0Ljk5MDA2LDEyMS41MzQ2LDIyLjgNCjMzNiwyMDEzLjU4MywyNy4zLDMzNy42MDE2LDYsMjQuOTY0MzEsMTIxLjU0MDYzLDM2LjUNCjMzNywyMDEyLjgzMyw1LjEsMTg2Ny4yMzMsMiwyNC45ODQwNywxMjEuNTE3NDgsMzUuNg0KMzM4LDIwMTIuODMzLDMxLjMsNjAwLjg2MDQsNSwyNC45Njg3MSwxMjEuNTQ2NTEsMzAuOQ0KMzM5LDIwMTIuOTE3LDMxLjUsMjU4LjE4Niw5LDI0Ljk2ODY3LDEyMS41NDMzMSwzNi4zDQozNDAsMjAxMy4zMzMsMS43LDMyOS45NzQ3LDUsMjQuOTgyNTQsMTIxLjU0Mzk1LDUwLjQNCjM0MSwyMDEzLjMzMywzMy42LDI3MC44ODk1LDAsMjQuOTcyODEsMTIxLjUzMjY1LDQyLjkNCjM0MiwyMDEzLjAwMCwxMyw3NTAuMDcwNCwyLDI0Ljk3MzcxLDEyMS41NDk1MSwzNw0KMzQzLDIwMTIuNjY3LDUuNyw5MC40NTYwNiw5LDI0Ljk3NDMzLDEyMS41NDMxLDUzLjUNCjM0NCwyMDEzLjAwMCwzMy41LDU2My4yODU0LDgsMjQuOTgyMjMsMTIxLjUzNTk3LDQ2LjYNCjM0NSwyMDEzLjUwMCwzNC42LDMwODUuMTcsMCwyNC45OTgsMTIxLjUxNTUsNDEuMg0KMzQ2LDIwMTIuNjY3LDAsMTg1LjQyOTYsMCwyNC45NzExLDEyMS41MzE3LDM3LjkNCjM0NywyMDEzLjQxNywxMy4yLDE3MTIuNjMyLDIsMjQuOTY0MTIsMTIxLjUxNjcsMzAuOA0KMzQ4LDIwMTMuNTgzLDE3LjQsNjQ4OC4wMjEsMSwyNC45NTcxOSwxMjEuNDczNTMsMTEuMg0KMzQ5LDIwMTIuODMzLDQuNiwyNTkuNjYwNyw2LDI0Ljk3NTg1LDEyMS41NDUxNiw1My43DQozNTAsMjAxMi43NTAsNy44LDEwNC44MTAxLDUsMjQuOTY2NzQsMTIxLjU0MDY3LDQ3DQozNTEsMjAxMy4wMDAsMTMuMiw0OTIuMjMxMyw1LDI0Ljk2NTE1LDEyMS41MzczNyw0Mi4zDQozNTIsMjAxMi44MzMsNCwyMTgwLjI0NSwzLDI0Ljk2MzI0LDEyMS41MTI0MSwyOC42DQozNTMsMjAxMi44MzMsMTguNCwyNjc0Ljk2MSwzLDI0Ljk2MTQzLDEyMS41MDgyNywyNS43DQozNTQsMjAxMy41MDAsNC4xLDIxNDcuMzc2LDMsMjQuOTYyOTksMTIxLjUxMjg0LDMxLjMNCjM1NSwyMDEzLjQxNywxMi4yLDEzNjAuMTM5LDEsMjQuOTUyMDQsMTIxLjU0ODQyLDMwLjENCjM1NiwyMDEzLjI1MCwzLjgsMzgzLjg2MjQsNSwyNC45ODA4NSwxMjEuNTQzOTEsNjAuNw0KMzU3LDIwMTIuODMzLDEwLjMsMjExLjQ0NzMsMSwyNC45NzQxNywxMjEuNTI5OTksNDUuMw0KMzU4LDIwMTMuNDE3LDAsMzM4Ljk2NzksOSwyNC45Njg1MywxMjEuNTQ0MTMsNDQuOQ0KMzU5LDIwMTMuMTY3LDEuMSwxOTMuNTg0NSw2LDI0Ljk2NTcxLDEyMS41NDA4OSw0NS4xDQozNjAsMjAxMy41MDAsNS42LDI0MDguOTkzLDAsMjQuOTU1MDUsMTIxLjU1OTY0LDI0LjcNCjM2MSwyMDEyLjY2NywzMi45LDg3LjMwMjIyLDEwLDI0Ljk4MywxMjEuNTQwMjIsNDcuMQ0KMzYyLDIwMTMuMDgzLDQxLjQsMjgxLjIwNSw4LDI0Ljk3MzQ1LDEyMS41NDA5Myw2My4zDQozNjMsMjAxMy40MTcsMTcuMSw5NjcuNCw0LDI0Ljk4ODcyLDEyMS41MzQwOCw0MA0KMzY0LDIwMTMuNTAwLDMyLjMsMTA5Ljk0NTUsMTAsMjQuOTgxODIsMTIxLjU0MDg2LDQ4DQozNjUsMjAxMy40MTcsMzUuMyw2MTQuMTM5NCw3LDI0Ljk3OTEzLDEyMS41MzY2NiwzMy4xDQozNjYsMjAxMi45MTcsMTcuMywyMjYxLjQzMiw0LDI0Ljk2MTgyLDEyMS41MTIyMiwyOS41DQozNjcsMjAxMi43NTAsMTQuMiwxODAxLjU0NCwxLDI0Ljk1MTUzLDEyMS41NTI1NCwyNC44DQozNjgsMjAxMi44MzMsMTUsMTgyOC4zMTksMiwyNC45NjQ2NCwxMjEuNTE1MzEsMjAuOQ0KMzY5LDIwMTMuNDE3LDE4LjIsMzUwLjg1MTUsMSwyNC45NzU0NCwxMjEuNTMxMTksNDMuMQ0KMzcwLDIwMTIuNjY3LDIwLjIsMjE4NS4xMjgsMywyNC45NjMyMiwxMjEuNTEyMzcsMjIuOA0KMzcxLDIwMTIuNzUwLDE1LjksMjg5LjMyNDgsNSwyNC45ODIwMywxMjEuNTQzNDgsNDIuMQ0KMzcyLDIwMTMuNTAwLDQuMSwzMTIuODk2Myw1LDI0Ljk1NTkxLDEyMS41Mzk1Niw1MS43DQozNzMsMjAxMy4wMDAsMzMuOSwxNTcuNjA1Miw3LDI0Ljk2NjI4LDEyMS41NDE5Niw0MS41DQozNzQsMjAxMy4wODMsMCwyNzQuMDE0NCwxLDI0Ljk3NDgsMTIxLjUzMDU5LDUyLjINCjM3NSwyMDEzLjI1MCw1LjQsMzkwLjU2ODQsNSwyNC45NzkzNywxMjEuNTQyNDUsNDkuNQ0KMzc2LDIwMTMuMjUwLDIxLjcsMTE1Ny45ODgsMCwyNC45NjE2NSwxMjEuNTUwMTEsMjMuOA0KMzc3LDIwMTMuNDE3LDE0LjcsMTcxNy4xOTMsMiwyNC45NjQ0NywxMjEuNTE2NDksMzAuNQ0KMzc4LDIwMTMuMzMzLDMuOSw0OS42NjEwNSw4LDI0Ljk1ODM2LDEyMS41Mzc1Niw1Ni44DQozNzksMjAxMy4zMzMsMzcuMyw1ODcuODg3Nyw4LDI0Ljk3MDc3LDEyMS41NDYzNCwzNy40DQozODAsMjAxMy4zMzMsMCwyOTIuOTk3OCw2LDI0Ljk3NzQ0LDEyMS41NDQ1OCw2OS43DQozODEsMjAxMy4zMzMsMTQuMSwyODkuMzI0OCw1LDI0Ljk4MjAzLDEyMS41NDM0OCw1My4zDQozODIsMjAxMy40MTcsOCwxMzIuNTQ2OSw5LDI0Ljk4Mjk4LDEyMS41Mzk4MSw0Ny4zDQozODMsMjAxMy4wMDAsMTYuMywzNTI5LjU2NCwwLDI0LjkzMjA3LDEyMS41MTU5NywyOS4zDQozODQsMjAxMi42NjcsMjkuMSw1MDYuMTE0NCw0LDI0Ljk3ODQ1LDEyMS41Mzg4OSw0MC4zDQozODUsMjAxMi43NTAsMTYuMSw0MDY2LjU4NywwLDI0Ljk0Mjk3LDEyMS41MDM0MiwxMi45DQozODYsMjAxMy4wMDAsMTguMyw4Mi44ODY0MywxMCwyNC45ODMsMTIxLjU0MDI2LDQ2LjYNCjM4NywyMDEyLjgzMywwLDE4NS40Mjk2LDAsMjQuOTcxMSwxMjEuNTMxNyw1NS4zDQozODgsMjAxMy4yNTAsMTYuMiwyMTAzLjU1NSwzLDI0Ljk2MDQyLDEyMS41MTQ2MiwyNS42DQozODksMjAxMy41MDAsMTAuNCwyMjUxLjkzOCw0LDI0Ljk1OTU3LDEyMS41MTM1MywyNy4zDQozOTAsMjAxMy4yNTAsNDAuOSwxMjIuMzYxOSw4LDI0Ljk2NzU2LDEyMS41NDIzLDY3LjcNCjM5MSwyMDEzLjUwMCwzMi44LDM3Ny44MzAyLDksMjQuOTcxNTEsMTIxLjU0MzUsMzguNg0KMzkyLDIwMTMuNTgzLDYuMiwxOTM5Ljc0OSwxLDI0Ljk1MTU1LDEyMS41NTM4NywzMS4zDQozOTMsMjAxMy4wODMsNDIuNyw0NDMuODAyLDYsMjQuOTc5MjcsMTIxLjUzODc0LDM1LjMNCjM5NCwyMDEzLjAwMCwxNi45LDk2Ny40LDQsMjQuOTg4NzIsMTIxLjUzNDA4LDQwLjMNCjM5NSwyMDEzLjUwMCwzMi42LDQxMzYuMjcxLDEsMjQuOTU1NDQsMTIxLjQ5NjMsMjQuNw0KMzk2LDIwMTIuOTE3LDIxLjIsNTEyLjU0ODcsNCwyNC45NzQsMTIxLjUzODQyLDQyLjUNCjM5NywyMDEyLjY2NywzNy4xLDkxOC42MzU3LDEsMjQuOTcxOTgsMTIxLjU1MDYzLDMxLjkNCjM5OCwyMDEzLjQxNywxMy4xLDExNjQuODM4LDQsMjQuOTkxNTYsMTIxLjUzNDA2LDMyLjINCjM5OSwyMDEzLjQxNywxNC43LDE3MTcuMTkzLDIsMjQuOTY0NDcsMTIxLjUxNjQ5LDIzDQo0MDAsMjAxMi45MTcsMTIuNywxNzAuMTI4OSwxLDI0Ljk3MzcxLDEyMS41Mjk4NCwzNy4zDQo0MDEsMjAxMy4yNTAsMjYuOCw0ODIuNzU4MSw1LDI0Ljk3NDMzLDEyMS41Mzg2MywzNS41DQo0MDIsMjAxMy4wODMsNy42LDIxNzUuMDMsMywyNC45NjMwNSwxMjEuNTEyNTQsMjcuNw0KNDAzLDIwMTIuODMzLDEyLjcsMTg3LjQ4MjMsMSwyNC45NzM4OCwxMjEuNTI5ODEsMjguNQ0KNDA0LDIwMTIuNjY3LDMwLjksMTYxLjk0Miw5LDI0Ljk4MzUzLDEyMS41Mzk2NiwzOS43DQo0MDUsMjAxMy4zMzMsMTYuNCwyODkuMzI0OCw1LDI0Ljk4MjAzLDEyMS41NDM0OCw0MS4yDQo0MDYsMjAxMi42NjcsMjMsMTMwLjk5NDUsNiwyNC45NTY2MywxMjEuNTM3NjUsMzcuMg0KNDA3LDIwMTMuMTY3LDEuOSwzNzIuMTM4Niw3LDI0Ljk3MjkzLDEyMS41NDAyNiw0MC41DQo0MDgsMjAxMy4wMDAsNS4yLDI0MDguOTkzLDAsMjQuOTU1MDUsMTIxLjU1OTY0LDIyLjMNCjQwOSwyMDEzLjQxNywxOC41LDIxNzUuNzQ0LDMsMjQuOTYzMywxMjEuNTEyNDMsMjguMQ0KNDEwLDIwMTMuMDAwLDEzLjcsNDA4Mi4wMTUsMCwyNC45NDE1NSwxMjEuNTAzODEsMTUuNA0KNDExLDIwMTIuNjY3LDUuNiw5MC40NTYwNiw5LDI0Ljk3NDMzLDEyMS41NDMxLDUwDQo0MTIsMjAxMy4yNTAsMTguOCwzOTAuOTY5Niw3LDI0Ljk3OTIzLDEyMS41Mzk4Niw0MC42DQo0MTMsMjAxMy4wMDAsOC4xLDEwNC44MTAxLDUsMjQuOTY2NzQsMTIxLjU0MDY3LDUyLjUNCjQxNCwyMDEzLjUwMCw2LjUsOTAuNDU2MDYsOSwyNC45NzQzMywxMjEuNTQzMSw2My45DQo=" download="Real estate.csv">Download Real estate.csv</a>
```


```r
houses <- read.csv("Real estate.csv", header = TRUE)
colnames(houses) <- c('date', 'age', 'distance_to_transit', 'num_stores', 'lat', 'long', 'price_per_unit_area')
```

Let's fit a multiple linear regression model to predict price per unit area using a house's age, its distance to the nearest transit station, and the number of nearby convenience stores.  Intuitively, age affects a home's value, while the other two variables have to do with the value of the location and neighborhood.  


```r
my.lm <- lm(price_per_unit_area~age+distance_to_transit+num_stores, data = houses)
summary(my.lm)
```

```
## 
## Call:
## lm(formula = price_per_unit_area ~ age + distance_to_transit + 
##     num_stores, data = houses)
## 
## Residuals:
##       Min        1Q    Median        3Q       Max 
## -0.013025 -0.003845 -0.000584  0.002303  0.052341 
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)          1.206e+02  3.203e+00   37.65   <2e-16 ***
## age                  4.619e-04  1.591e-03    0.29    0.772    
## distance_to_transit -3.774e-05  3.932e-05   -0.96    0.338    
## num_stores          -9.802e-06  3.555e-07  -27.57   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.0091 on 410 degrees of freedom
## Multiple R-squared:  0.651,	Adjusted R-squared:  0.6484 
## F-statistic: 254.9 on 3 and 410 DF,  p-value: < 2.2e-16
```

The fitted regression line is
\[\text{price per unit area} = 120.6 + 0.0004619 \times \text{age} - 0.00003774 \times \text{distance to transit} - 0.000009802 \times \text{number of stores}.\]
However, t-tests suggest that neither the inclusion of age nor distance to transit in the model are improving the price predictions (p-values are 0.772 and 0.338).


```r
X <- model.matrix(price_per_unit_area~age+distance_to_transit+num_stores, data = houses)
hat.sigma2 <- summary(my.lm)$sigma^2
cov.beta <- hat.sigma2*solve(t(X)%*%X)
cov.beta[2,3]/sqrt(cov.beta[2,2]*cov.beta[3,3])
```

```
## [1] -0.01602387
```

```r
cov.beta[2,4]/sqrt(cov.beta[2,2]*cov.beta[4,4])
```

```
## [1] -0.06045947
```

```r
cov.beta[3,4]/sqrt(cov.beta[3,3]*cov.beta[4,4])
```

```
## [1] -0.0246031
```

Above we computed the estimated correlation between the coefficients of age, distance to transit, and number of stores.  These estimated correlations are all close to zero, meaning the estimated coefficients are not correlated.  When estimated coefficients are strongly correlated, it is challenging to interpret the multiple linear regression model.   For example, the estimate of $\hat\beta_1 = 0.0004619$ means that for a one unit increase in age, the home price by unit area increases by 0.0004619 units **provided all other covariates are held constant**.  If the covariates are not correlated, this explanation makes sense.  However, if two covariates are strongly, say, positively correlated, then when one goes up it must be that the other goes up as well, so it is not reasonable to assume one can be changed while the other is held constant.  Correlation of the covariates is called *multicollinearity* and it is important to check for multicollinearity before attempting to interpret coefficients as above.

<br><br>

Note that the summary of the lm function call includes an F test result in the last line.  It says "F-statistic: 254.9 on 3 and 410 DF,  p-value: < 2.2e-16".  This is called the *model F test* and it is a partial F test of the hypothesis that only the intercept coefficient is non zero, i.e. $\beta_0 \ne 0$ but $\beta_j = 0$ for all $j > 0$.  We can match this F test result by fitting the "intercept-only" model and comparing SSEs:


```r
my.lm.int <- lm(price_per_unit_area~1, data = houses)
SSE.R <- sum(my.lm.int$residuals^2)
SSE.F <- sum(my.lm$residuals^2)
n <- length(houses$price_per_unit_area)

F <- ((SSE.R - SSE.F)/3)/(SSE.F / (n-4))
F
```

```
## [1] 254.923
```

```r
1-pf(F,3,n-4)
```

```
## [1] 0
```



