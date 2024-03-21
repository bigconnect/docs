# npm Bundler

The npm-bundler is a bundler (like [Webpack](https://webpack.github.io/) or [Browserify](http://browserify.org/)) that targets MWARE Portal as a platform and assumes you’re using your npm packages from widgets (as opposed to typical web applications).

The workflow for running npm packages inside widgets is slightly different from standard bundlers. Instead of bundling the JavaScript in a single file, you must _link_ all packages together in the browser when the full web page is assembled. Widgets can then share common versions of modules rather than each loading its own copy. The npm-bundler handles this for you.

NOTE

You can also find information for the npm-bundler in the project’s Wiki.

### How the npm Bundler Works Internally <a href="#how-the-liferay-npm-bundler-works-internally" id="how-the-liferay-npm-bundler-works-internally"></a>

The npm-bundler takes a widget project and outputs its files (including npm packages) to a build folder, so the standard widget build (Gradle) can produce an OSGi bundle. You can learn more about the build folder’s structure in OSGi Bundles and npm Package Structure reference.

The npm-bundler uses the process below to create the OSGi bundle:

1. Copy the project’s `package.json` file to the output directory.
2. Traverse the project’s dependency tree to determine its dependencies.
3.  For the project,

    a. Run the source files, specified in the `.npmbundlerrc` configuration, through the rules.

    b. Pre-process the project’s package with any configured plugins.

    c. Run [Babel](https://babeljs.io/) with configured plugins for each `.js` file inside the project.

    d. Post-process the project package with any configured plugins.
4.  For each npm package dependency,

    a. Copy the npm package to the output folder and prefix the bundle’s name to it. Note that the bundler stores packages in a plain _bundle-name$package_@_version_ format, rather than the standard node\_modules tree format. To determine what is copied, the bundler invokes a plugin to filter the package file list.

    b. Run rules on the package files.

    c. Pre-process the npm package with any configured plugins.

    d. Run [Babel](https://babeljs.io/) with configured plugins for each `.js` file inside the npm package.

    e. Post-process the npm package with any configured plugins.

The only difference between the pre-process and post-process steps are when they are run (before or after Babel is run, respectively). During this workflow, npm-bundler calls all the configured plugins so they can perform transformations on the npm packages (for instance, modifying their `package.json` files or deleting or moving files).

NOTE

The pre, post, and Babel phases were designed for the old mode of operation (See the Migrating Your Project to Use the New Mode for more information) and they will gradually be replaced with rules for the new mode.

This reference covers the npm-bundler’s configuration, default presets, format, and more.
