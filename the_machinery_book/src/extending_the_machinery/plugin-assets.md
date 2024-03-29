# Plugin assets

When you put a plugin in the `plugin` folder, it will be loaded every time you start The Machinery
and used by all projects. This is convenient, but sometimes you want plugins that are project
specific, e.g., the gameplay code for a particular game.

There are two ways of doing this.

First, you could create a separate The Machinery executable folder for that specific project. Just
make a copy of the The Machinery folder and add any plugins you need. Whenever you want to work on
that project, make sure to start that executable instead of the standard one.

In addition to adding project specific plugins, this method also lets you do additional things,
such as using different versions of The Machinery for different projects and remove any of the
standard plugins that you *don't* need in your project.

The second method is to store plugins as *assets* in the project itself. To do this, create a *New
Plugin* in the *Asset Browser* and set the DLL path of the plugin to your DLL. We call this a 
*Plugin Asset*.

The *Plugin Assets* will be loaded whenever you open the project and unloaded whenever you close
the project. Since the plugin is distributed with the project, if you send the project to someone,
they will automatically get the plugin too -- they don't have to manually install into their
`plugin` folder. This can be a convenient way of distributing plugins.

> **WARNING: Security Warning**
>
>  Since plugin assets can contain arbitrary code and there is no sandboxing, when you run a
>     plugin asset, it will have full access to your machine. Therefore, you should only run plugin
>     assets from trusted sources. When you open a project that contains plugin assets, you will 
>     be asked if you want to allow the code to run on your machine or not. You should only click
>     *[Allow]* if you trust the author of the project.

> **NOTE: Version Issues**
>
>  Since The Machinery is still in early adopters mode and doesn't have a stable API, plugins will only work
>     with the specific version they are developed for. If you send a plugin to someone else
>     (for example as a plugin asset in a project), you must make sure that they use the exact same
>     version of The Machinery. Otherwise, the plugin will most likely crash.

You hot-reload plugin assets by checking the *Import When Changed* checkbox in the plugin
properties. If checked, the editor will monitor the plugin's import path for changes and if it
detects a file change, it will reimport the plugin.
