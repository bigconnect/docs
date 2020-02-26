# Notebooks

The notebook extends the console-based approach to interactive computing in a qualitatively new direction, providing a web-based application suitable for capturing the whole computation process: developing, documenting, and executing code, as well as communicating the results. The Jupyter notebook combines two components:

**A web application**: a browser-based tool for interactive authoring of documents which combine explanatory text, mathematics, computations and their rich media output.

**Notebook documents**: a representation of all content visible in the web application, including inputs and outputs of the computations, explanatory text, mathematics, images, and rich media representations of objects.

## Main features of the web application

* In-browser editing for code, with automatic syntax highlighting, indentation, and tab completion/introspection.
* The ability to execute code from the browser, with the results of computations attached to the code which generated them.
* Displaying the result of computation using rich media representations, such as HTML, LaTeX, PNG, SVG, etc. For example, publication-quality figures rendered by the [matplotlib](https://matplotlib.org/) library, can be included inline.
* In-browser editing for rich text using the [Markdown](https://daringfireball.net/projects/markdown/syntax) markup language, which can provide commentary for the code, is not limited to plain text.
* The ability to easily include mathematical notation within markdown cells using LaTeX, and rendered natively by [MathJax](https://www.mathjax.org/).

## Notebook documents

Notebook documents contains the inputs and outputs of a interactive session as well as additional text that accompanies the code but is not meant for execution. In this way, notebook files can serve as a complete computational record of a session, interleaving executable code with explanatory text, mathematics, and rich representations of resulting objects. These documents are internally [JSON](https://en.wikipedia.org/wiki/JSON) files and are saved with the `.ipynb` extension. Since JSON is a plain text format, they can be version-controlled and shared with colleagues.

Notebooks may be exported to a range of static formats, including HTML \(for example, for blog posts\), reStructuredText, LaTeX, PDF, and slide shows, via the [nbconvert](https://nbconvert.readthedocs.io/en/latest/) command.

Furthermore, any `.ipynb` notebook document available from a public URL can be shared via the [Jupyter Notebook Viewer](https://jupyter-notebook.readthedocs.io/en/stable/nbviewer) \([nbviewer](https://nbviewer.jupyter.org/)\). This service loads the notebook document from the URL and renders it as a static web page. The results may thus be shared with a colleague, or as a public blog post, without other users needing to install the Jupyter notebook themselves. In effect, [nbviewer](https://nbviewer.jupyter.org/) is simply [nbconvert](https://nbconvert.readthedocs.io/en/latest/) as a web service, so you can do your own static conversions with nbconvert, without relying on nbviewer.

## Creating a new notebook document

A new notebook may be created at any time, either from the dashboard, or using the File ‣ New menu option from within an active notebook. The new notebook is created within the same directory and will open in a new browser tab. It will also be reflected as a new entry in the notebook list on the dashboard.

![](../.gitbook/assets/image%20%28172%29.png)

## Opening notebooks

An open notebook has **exactly one** interactive session connected to a kernel, which will execute code sent by the user and communicate back results. This kernel remains active if the web browser window is closed, and reopening the same notebook from the dashboard will reconnect the web application to the same kernel. In the dashboard, notebooks with an active kernel have a `Shutdown` button next to them, whereas notebooks without an active kernel have a `Delete` button in its place.

## Notebook user interface

When you create a new notebook document, you will be presented with the **notebook name**, a **menu bar**, a **toolbar** and an empty **code cell**.

![](../.gitbook/assets/image%20%28218%29.png)

**Notebook name**: The name displayed at the top of the page, next to the Jupyter logo, reflects the name of the `.ipynb` file. Clicking on the notebook name brings up a dialog which allows you to rename it. Thus, renaming a notebook from “Untitled0” to “My first notebook” in the browser, renames the `Untitled0.ipynb` file to `My first notebook.ipynb`.

**Menu bar**: The menu bar presents different options that may be used to manipulate the way the notebook functions.

**Toolbar**: The tool bar gives a quick way of performing the most-used operations within the notebook, by clicking on an icon.

**Code cell**: the default type of cell; read on for an explanation of cells.

## Notebooks and privacy

Because you use Jupyter in a web browser, some people are understandably concerned about using it with sensitive data. However, if you followed the standard [install instructions](https://jupyter.readthedocs.io/en/latest/install.html), Jupyter is actually running on your own computer. If the URL in the address bar starts with `http://localhost:` or `http://127.0.0.1:`, it’s your computer acting as the server. Jupyter doesn’t send your data anywhere else—and as it’s open source, other people can check that we’re being honest about this.

You can also use Jupyter remotely: your company or university might run the server for you, for instance. If you want to work with sensitive data in those cases, talk to your IT or data protection staff about it.

We aim to ensure that other pages in your browser or other users on the same computer can’t access your notebook server. See [Security in the Jupyter notebook server](https://jupyter-notebook.readthedocs.io/en/stable/security.html#server-security) for more about this.

