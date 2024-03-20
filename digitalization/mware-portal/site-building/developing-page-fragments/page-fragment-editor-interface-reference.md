# Page Fragment Editor Interface Reference

The Page Fragment editor’s interface is organized into two tabs:

* Code Editor
* Configuration

The sections below cover how to use these portions of the interface. See Developing Page Fragments to learn how to develop Page Fragments.

### Code Editor <a href="#code-editor" id="code-editor"></a>

The code editor is split into four panes:

**HTML:** Supports standard HTML, along with Portal specific tags and FreeMarker (using the alternative (square bracket) syntax) that can be used to add dynamic behavior.

TIP

Type `$(` to begin using auto-completion to find variable names. Type `[@` to use auto-completion to find taglib names.

**CSS:** Supports standard CSS.

**JavaScript:** Supports standard JavaScript and JQuery. You can also access configuration values in the FreeMarker context in the JavaScript pane.

**Preview:** provides a live preview of the component that updates as you write code. You can switch between desktop, mobile, tablet, and expanded views.

![The Fragments editor provides an environment for creating all the parts of a Fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/developer-guide/reference/fragments/page-fragment-editor-interface-reference/images/01.png)

### Configuration <a href="#configuration" id="configuration"></a>

NOTE

Defining configurations for Page Fragments is available in versions 7.2 +.

The Configuration tab provides input for adding configuration options to the Page Fragment’s Configuration Menu. This is equivalent to the `configuration.json` file that the Fragments Toolkit produces. For example, you can add a selector to your Page Fragment’s configuration options that lets users choose a color for the Page Fragment’s heading. Defining configuration options for a Page Fragment gives it more flexibility, reducing the number of Page Fragments you must maintain.

NOTE

Changes made to the configuration are automatically made available to the code editor. If your configuration is invalid, you can’t publish your Page Fragment. Be sure to always have a valid JSON configuration before previewing or publishing Page Fragments.

Configuration values are made available to the Page Fragment through the FreeMarker context in the HTML pane. You can combine configuration options with conditional values in the HTML to create dynamic experiences for the user. You can also access these values via JavaScript. See Making Page Fragments Configurable for more information.

\
