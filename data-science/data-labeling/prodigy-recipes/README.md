# Recipes

A Prodigy recipe is a Python function that can be run via the command line. Prodigy comes with lots of useful recipes, and it's very easy to write your own. All you have to do is add the `@prodigy.recipe` decorator around your function, which should return a dictionary of components, specifying the stream of examples, and optionally the web view, database, progress calculator, and callbacks on update, load and exit.

Recipes don't have to start the web server – you can also use the recipe decorator as a quick way to make your Python function into a command-line utility. This is useful for batch training, database management, and data exploration. Prodigy comes with **built-in recipes** of the following categories:

| Recipe | Description |
| :--- | :--- |
| TEACH | Interactively annotate and train models. |
| EVALUATE | Test models and interactively create evaluation sets. |
| EXPLORE | Preview data and annotations from the command line. |
| MANAGE | Organise your data sets and annotation projects. |

{% page-ref page="prodigy-named-entity-recognition.md" %}

{% page-ref page="prodigy-text-classification.md" %}

{% page-ref page="prodigy-part-of-speech.md" %}

{% page-ref page="prodigy-dependency-parsing.md" %}

{% page-ref page="prodigy-terms.md" %}

{% page-ref page="prodigy-images.md" %}

