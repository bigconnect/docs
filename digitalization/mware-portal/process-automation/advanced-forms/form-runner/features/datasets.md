# Datasets

### Introduction <a href="#introduction" id="introduction"></a>

\[SINCE Orbeon Forms 2017.1]

A dataset is a mutable set of data, usually returned by a service and stored in the form in XML format.

A dataset is _transient_ at this point:

* It is not saved alongside the data in the database.
* It is not passed when navigating between the "edit" and "view" modes.

Instead, a dataset lives as long as the user stays on the given form page.

### Creating a dataset <a href="#creating-a-dataset" id="creating-a-dataset"></a>

You create a dataset using the [Actions Editor](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions). The data returned by the service is stored into the dataset in XML format.

A dataset is identified by a name. Multiple actions calling services can store data into the same dataset. The action which last updates the dataset overwrites the entire content of the dataset.

### Using data from the dataset <a href="#using-data-from-the-dataset" id="using-data-from-the-dataset"></a>

Storing data into the dataset only makes sense if you make use of that data at a later point.

You do so using the [`fr:dataset()`](https://doc.orbeon.com/xforms/xpath/extension-functions/extension-form-runner#frdataset) function. You can use this function from [formulas](https://doc.orbeon.com/form-builder/formulas), including validations, visibility, etc. For example:

Copy

```
fr:dataset('activity-dataset')/foo/bar
```
