# Partial Dependence Plots

For models that include only numerical values, you can view a Partial Dependence Plot \(PDP\) for that model. This provides a graphical representation of the marginal effect of a variable on the class probability \(classification\) or response \(regression\).

### Viewing Partial Dependence Plots

1. To view a PDP for a model, click the Score dropdown at the top of the Flow UI, then select Partial Dependence Plots.

![](../../../../.gitbook/assets/image%20%28189%29.png)

2. Specify the Model and Frame that you want to use to retrieve the plots, and specify the number of bins \(levels that PDP will compute\). Note that more levels will result in slower speeds. By default, the top 10 features are used to build the plot. Alternatively, you can click the **Select Column?** button to build a plot based on a specified set of columns. Click **Compute** when you are done.

![](../../../../.gitbook/assets/image%20%2829%29.png)

3. After the job is finished, click **View** to see the plots.

![](../../../../.gitbook/assets/image%20%28146%29.png)



