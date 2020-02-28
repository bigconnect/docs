# Configuration

## General

When you first run Prodigy, it will create a folder `.prodigy` in your home directory. By default, this will be the location where Prodigy looks for its global configuration file, `prodigy.json`. You can change this directory via the environment variable `PRODIGY_HOME`:

```text
PRODIGY_HOME=/custom/prodigy/home
```

When you run Prodigy, it will first check if a **global configuration file** exists. It will also check the **current working directory** for a `prodigy.json` or `.prodigy.json`. This allows you to overwrite specific settings on a project-by-project basis.

```javascript
PRODIGY.JSON
{
    "theme": "basic",
    "custom_theme": {},
    "batch_size": 10,
    "port": 8080,
    "host": "localhost",
    "cors": true,
    "db": "sqlite",
    "db_settings": {},
    "api_keys": {},
    "auto_create": true,
    "auto_exclude_current": true,
    "instant_submit": false,
    "show_stats": false,
    "hide_meta": false,
    "show_flag": false,
    "instructions": false,
    "swipe": false,
    "split_sents_threshold": false,
    "diff_style": "words",
    "html_template": false,
    "global_css": null,
    "javascript": null,
    "writing_dir": "ltr",
    "hide_true_newline_tokens": false,
    "ner_manual_require_click": false,
    "ner_manual_label_style": "list",
    "choice_style": "single",
    "choice_auto_accept": false,
    "darken_image": 0,
    "show_bounding_box_center": false,
    "preview_bounding_boxes": false,
    "shade_bounding_boxes": false
}
```

{% hint style="info" %}
It's recommended to use the global config file in your Prodigy home for personal settings, i.e. the theme and theme modifications of your choice, or database credentials. Adding a `.prodigy.json` or `prodigy.json` to your project lets you overwrite and share the project settings with others – for example, the batch size or database to use.
{% endhint %}

| SETTING | DESCRIPTION | DEFAULT |
| :--- | :--- | :--- |
| `theme` | Name of UI theme to use. | `"basic"` |
| `custom_theme` | Custom UI theme overrides keyed by name. | `{}` |
| `batch_size` | Number of tasks to return to and receive back from the web app at once. A low batch size means more frequent updates. | `10` |
| `port` | Port to use for serving the web application. | `8080` |
| `host` | Host to use for serving the web application. | `"localhost"` |
| `cors` | Enable or disable [cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) \(CORS\) to allow the REST API to receive requests from other domains. | `true` |
| `db` | Name of database to use.  | `"sqlite"` |
| `db_settings` | Additional settings for the respective database. | `{}` |
| `api_keys` | Live API keys, keyed by API loader ID. | `{}` |
| `auto_create` | Automatically create dataset if it doesn't exist in database. | `true` |
| `auto_exclude_current` | Automatically exclude examples already present in current dataset. | `true` |
| `instant_submit` | Instantly submit a task after it's answered in the app, skipping the history. | `false` |
| `show_stats` | Show additional stats, like decision counts, in the sidebar. | `false` |
| `hide_meta` | Hide the meta information displayed on annotation cards. | `false` |
| `show_flag` | Show a flag icon in the top right corner that lets you bookmark a task for later. Will add `"flagged": true` to the task. | `false` |
| `instructions` | Path to a text file with instructions for the annotator \(HTML allowed\). Will be displayed as a help modal in the UI. | `false` |
| `swipe` | Enable swipe gestures on touch devices \(left for ACCEPT, right for REJECT\). | `false` |
| `split_sents_threshold` | Minimum character length of a text to be split by the `split_sentences` preprocessor, mostly used in NER recipes. If `false`, all multi-sentence texts will be split. | `false` |
| `diff_style` | Style for visual diffs. `"words"`, `"chars"` or `"sentences"`. | `"words"` |
| `html_template` | Optional [Mustache](https://mustache.github.io/) template for content in the `html` annotation interface. All task properties are available as variables. | `false` |
| `global_css` | CSS overrides added to the global scope. | `null` |
| `javascript` | Custom JavaScript added to the global scope. | `null` |
| `writing_dir` | Writing direction. Mostly important for manual text annotation interfaces. | `"ltr"` |
| `hide_true_newline_tokens` | Don't add real line breaks to tokens in manual annotation mode and only use `↵` symbols. | `false` |
| `ner_manual_require_click` | In manual NER annotation mode, don't auto-add the selection as an entity and require an additional button click to convert the selection to an entity. | `false` |
| `ner_manual_label_style` | Style of label set in manual NER model. `"list"` for list of keyboard-accessible buttons or `"dropdown"`. | `"list"` |
| `choice_style` | Style of choice interface, `"single"` or `"multiple"`. | `"single"` |
| `choice_auto_accept` | Automatically accept a single-choice option when selecting it – without having to click the ACCEPT button. | `false` |
| `darken_image` | Darken an image in image detection or segmentation mode for better visibility of the coloured bounding boxes. For example, `0.25` will darken the image by 25%. | `0` |
| `show_bounding_box_center` | Show a dot marking the center of a bounding box. | `false` |
| `preview_bounding_boxes` | Also show image spans with `"hidden": true` in the background to preview other bounding boxes detected in the same image. This can help with making the annotation decision, because you get to preview potentially better analyses. | `false` |
| `shade_bounding_boxes` | Display bounding boxes with an opaque background color. Mostly used for annotating larger image segments. | `false` |

## Database setup

By default, Prodigy uses [SQLite](https://www.sqlite.org/) to store annotations in a simple database file in your Prodigy home directory. If you want to use the default database with its default settings, **no further configuration is required** and you can start using Prodigy straight away. Alternatively, you can choose to use Prodigy with a [MySQL](https://www.mysql.com/) or [PostgreSQL](https://www.postgresql.org/) database, or write your own custom recipe to plug in any other storage solution.

| DATABASE | ID | SETTINGS |
| :--- | :--- | :--- |
| **SQLite** | `sqlite` | `name` for database file name \(defaults to `"prodigy.db"`\), `path` for custom path \(defaults to Prodigy home directory\), plus [sqlite3](https://docs.python.org/2/library/sqlite3.html#sqlite3.connect) connection parameters. To only store the database in memory, use `":memory:"` as the database name. |
| **MySQL** | `mysql` | [MySQLdb](http://mysql-python.sourceforge.net/MySQLdb.html#some-mysql-examples) or [PyMySQL](https://github.com/PyMySQL/PyMySQL/blob/f08f01fe8a59e8acfb5f5add4a8fe874bec2a196/pymysql/connections.py#L494-L513) connection parameters. |
| **PostgreSQL** | `postgresql` | [psycopg2](http://initd.org/psycopg/docs/module.html#psycopg2.connect) connection parameters. |

```javascript
PRODIGY.JSON
{
    "db": "sqlite",
    "db_settings": {
        "sqlite": {},
        "mysql": {},
        "postgresql": {}
    }
}
```

#### WHY DO I NEED A DATABASE?

Prodigy uses the database to store all annotations by dataset and annotation session. Even if you're only using Prodigy with a model in the loop, you'll usually want a record of the collected annotations as a backup, or to use them as evaulation data. Prodigy is also very powerful without a model in the loop – for example to bootstrap word lists or collect feedback on the output of two models and generate evaluation data for machine translation or image classification.

