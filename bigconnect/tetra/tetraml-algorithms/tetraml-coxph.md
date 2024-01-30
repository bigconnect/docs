# Cox Proportional Hazards (CoxPH)

Cox proportional hazards models are the most widely used approach for modeling time to event data. As the name suggests, the _hazard function_, which computes the instantaneous rate of an event occurrence and is expressed mathematically as:

$$
h(t) = \lim_{\Delta t \downarrow 0} \frac{Pr[t \le T < t + \Delta t \mid T \ge t]}{\Delta t},
$$

is assumed to be the product of a _baseline hazard function_ and a _risk score_. Consequently, the hazard function for observation _i_  in a Cox proportional hazards model is defined as:

$$
h_i(t) = \lambda(t)\exp(\mathbf{x}_i^T\beta)
$$

where λ(t) is the baseline hazard function shared by all observations and $$\exp(\mathbf{x}_i^T\beta)$$ is the risk score for observation i, which is computed as the exponentiated linear combination of the covariate vector $$\mathbf{x}_i^T$$ using a coefficient vector β common to all observations.

This combination of a non-parametric baseline hazard function and a parametric risk score results in Cox proportional hazards models being described as _semi-parametric_. In addition, a simple rearrangement of terms shows that unlike generalized linear models, an intercept (constant) term in the risk score adds no value to the model fit, due to the inclusion of a baseline hazard function.

### Defining a CoxPH Model

* `model_id`: (Optional) Specify a custom name for the model to use as a reference. By default, TetraML automatically generates a destination key.
* `training_frame`: (Required) Specify the dataset used to build the model. **NOTE**: In Flow, if you click the **Build a model** button from the `Parse` cell, the training frame is entered automatically.
* `start_column`: (Optional) The name of an integer column in the **source** data set representing the start time. If supplied, the value of the **start\_column** must be strictly less than the **stop\_column** in each row.
* `stop_column`: (Required) The name of an integer column in the **source** data set representing the stop time.
* `y` / `event_column`: (Required) Specify the column to use as the dependent variable. The data can be numeric or categorical.
* `ignored_columns`: (Optional) Specify the column or columns to be excluded from the model. In Flow, click the checkbox next to a column name to add it to the list of columns excluded from the model. To add all columns, click the **All** button. To remove a column from the list of ignored columns, click the X next to the column name. To remove all columns from the list of ignored columns, click the **None** button. To search for a specific column, type the column name in the **Search** field above the column list. To only show columns with a specific percentage of missing values, specify the percentage in the **Only show columns with more than 0% missing values** field. To change the selections for the hidden columns, use the **Select Visible** or **Deselect Visible** buttons.
* `weights_column`: Specify a column to use for the observation weights, which are used for bias correction. The specified `weights_column` must be included in the specified `training_frame`.
* `offset_column`: Specify a column to use as the offset. **Note**: Offsets are per-row “bias values” that are used during model training. For Gaussian distributions, they can be seen as simple corrections to the response (y) column. Instead of learning to predict the response (y-row), the model learns to predict the (row) offset of the response column. For other distributions, the offset corrections are applied in the linearized space before applying the inverse link function to get the actual response values.
* `stratify_by`: A list of columns to use for stratification.
* `ties`: The approximation method for handling ties in the partial likelihood. This can be either **efron** (default) or **breslow**. See the _**Cox Proportional Hazards Model Details**_ section below for more information about these options.
* `init`: (Optional) Initial values for the coefficients in the model. This value defaults to 0.
* `lre_min`: A positive number to use as the minimum log-relative error (LRE) of subsequent log partial likelihood calculations to determine algorithmic convergence. The role this parameter plays in the stopping criteria of the model fitting algorithm is explained in the _**Cox Proportional Hazards Model Algorithm**_ section below. This value defaults to 9.
* `max_iterations`: A positive integer defining the maximum number of iterations during model training. The role this parameter plays in the stopping criteria of the model-fitting algorithm is explained in the _**Cox Proportional Hazards Model Algorithm**_ section below. This value defaults to 20.
* `interactions`: Specify a list of predictor column indices to interact. All pairwise combinations will be computed for this list.
* `interaction_pairs`: (Internal only.) When defining interactions, use this option to specify a list of pairwise column interactions (interactions between two variables). Note that this is different than `interactions`, which will compute all pairwise combinations of specified columns. This option is disabled by default.
* `export_checkpoints_dir`: Specify a directory to which generated models will automatically be exported.
* `single_node_mode`: Specify whether to run on a single node for fine-tuning of model parameters. Running on a single node reduces the effect of network overhead (for smaller datasets). This defaults to false.
