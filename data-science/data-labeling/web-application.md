# Web Application

When humans interact with machines, their experience is what decides about the success of the interaction. Prodigy helps you collect annotations faster and more efficiently by focusing on **one task at a time**, and reducing each annotation to a simple **binary decision**. This means you'll collect more user actions overall, giving you lots of smaller pieces to learn from, and a much tighter feedback loop between the human and the model.

![](../../.gitbook/assets/image%20%28444%29.png)

The web app includes a range of built-in annotation interfaces, key bindings and mobile swipe gestures and fully customisable UI themes. When you annotate a task, the decision – accept, reject or ignore – is added to the task as the `answer` key. The ten most recent decisions are shown in the sidebar and remain editable, either by clicking "undo", or by selecting them from the history. Annotated tasks are sent back to the REST API in batches. To save the current progress manually, you can always hit the save button, or press COMMAND+S or CTRL+S.

## Key bindings and swipe gestures

For more convenient annotation, you can use the following key bindings. You can also enable swipe gestures on touch devices, which will let you swipe left for ACCEPT and swipe right for REJECT.

## Statistics and progress

The sidebar helps you keep an overview of your annotation project and includes a progress widget showing the project's annotation count, progress and optional detailed statistics for the ACCEPT, REJECT and IGNORE decisions. To re-add an already annotated task to the front of the queue and change your decision, simply click on it in the history, or use the UNDO button or the BACKSPACE key.

![](../../.gitbook/assets/image%20%28386%29.png)

![](../../.gitbook/assets/image%20%28296%29.png)

## Flagging tasks

Flagging a task lets you to bookmark it for later, independent of the annotation decisions ACCEPT, REJECT and IGNORE. This can be useful to keep a record of seemingly strange predictions of your model, to investigate later on, or to save interesting examples you come across. Since flagging is mostly developer functionality, it's disabled by default and requires the `"show_flag"` setting. Flagged tasks will receive a `"flagged": true` entry, and can be exported separately by setting `--flagged-only` on `db-out`.

![](../../.gitbook/assets/image%20%28443%29.png)

## Annotation instructions

If you need to provide more detailed annotation instructions, you can use the `"instructions"` config setting to load in a text or HTML file. The instructions will then be displayed in a modal, accessible via the help icon at the top, or by pressing H.

![](../../.gitbook/assets/image%20%28350%29.png)

## Themes

Prodigy comes with three built-in theme options. To change the theme, simply edit the theme setting in your configuration file and restart the web app.

![basic \(default\)](../../.gitbook/assets/image%20%28150%29.png)

![eighties](../../.gitbook/assets/image%20%28143%29.png)



