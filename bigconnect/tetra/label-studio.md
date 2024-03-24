# Label Studio

Label Studio is a data labeling tool that supports multiple projects, users, and data types in one platform. It allows you to do the following:

* Perform different types of labeling with many data formats.
* Integrate Label Studio with machine learning models to supply predictions for labels (pre-labels), or perform continuous active learning.&#x20;

## Interface

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



## Labeling Workflow

1. Install Label Studio.
2. Start Label Studio.
3. Create accounts for Label Studio. Create an account to manage and set up labeling projects.
4. Set up the labeling project. Define the type of labeling to perform on the dataset and configure project settings.
5. Set up the labeling interface. Add the labels that you want annotators to apply and customize the labeling interface.
6. Import data as labeling tasks.
7. Label and annotate the data.
8. Export the labeled data or the annotations.

## Architecture

The component parts of Label Studio are available as modular extensible packages that you can integrate into your existing machine learning processes and tools.

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

| Module                    | Description                                                                                                                                                          |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Label Studio main app     | The main app with most of the backend code for Label Studio; used to perform data labeling.                                                                          |
| Label Studio frontend     | Located within the main app repo. `web/apps/labelstudio` acts as the central integration point for all frontend elements. `web/libs/editor` is the frontend library. |
| Data Manager              | Manage data and tasks for labeling. Located under `web/libs/datamanager` in the main app repo.                                                                       |
| Machine Learning Backends | Predict data labels at various parts of the labeling process.                                                                                                        |
