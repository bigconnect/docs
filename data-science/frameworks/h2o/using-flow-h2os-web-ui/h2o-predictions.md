# Predictions

After creating your model, click the key link for the model, then click the **Predict** button. Select the model to use in the prediction from the drop-down **Model:** menu and the data frame to use in the prediction from the drop-down **Frame:** menu, then click the **Predict** button.

![](../../../../.gitbook/assets/image%20%2818%29.png)

## Viewing Predictions

Click the **Assist Me!** button, then click the **getPredictions** link, or enter `getPredictions` in the cell in CS mode and press **Ctrl+Enter**. A list of the stored predictions displays. To view a prediction, click the **View** button to the right of the model name.

![](../../../../.gitbook/assets/image%20%28499%29.png)

You can also view predictions by clicking the drop-down **Score** menu and selecting **List All Predictions**.

## Interpreting the Gains/Lift Chart

The Gains/Lift chart evaluates the prediction ability of a binary classification model. The chart is computed using the prediction probability and the true response \(class\) labels. The accuracy of the classification model for a random sample is evaluated according to the results when the model is and is not used.

This information is particularly useful for direct marketing applications, for example. The gains/lift chart shows the effectiveness of the current model\(s\) compared to a baseline, allowing users to quickly identify the most useful model.

By default, H2O reports the Gains/Lift for all binary classification models if the following requirements are met:

* The training frame dataset must contain actual binary class labels.
* The prediction column used as the response must contain probabilities.
* For GLM, the visualization displays only when using `nfolds` \(for example, `nfolds=2`\).
* The model type cannot be K-means or PCA.

### How the Gains/Lift Chart is Built

To compute Gains/Lift, H2O applies the model to the original dataset to find the response probability. The data is divided into groups by quantile thresholds of the response probability. Note that the default number of groups is 16; if there are fewer than 16 unique probability values, then the number of groups is reduced to the number of unique quantile thresholds. For binning, H2O uses the following probabilities vector to create cut points \(0.99, 0.98, 0.97, 0.96, 0.95, 0.9, 0.85, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3, 0.2, 0.1, 0\).

For each group, the lift is calculated as the proportion of observations that are events \(targets\) in the group to the overall proportion of events \(targets\).

![](../../../../.gitbook/assets/image%20%28373%29.png)

In addition to the chart, a Gains/Lift table is also available. This table reports the following for each group:

* Threshold probability value
* Cumulative data fractions
* Response rates \(proportion of observations that are events in a group\)
* Cumulative response rate
* Event capture rate
* Cumulative capture rate
* Gain \(difference in percentages between the overall proportion of events and the observed proportion of observations that are events in the group\)
* Cumulative gain

![](../../../../.gitbook/assets/image%20%28460%29.png)

The _response\_rate_ column lists the likelihood of response, the _lift_ column lists the lift rate, and the _cumulative\_lift_ column provides the percentage of increase in response based on the lift.

