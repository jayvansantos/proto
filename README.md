# Proto 

*v1.0.1*

[Proto](https://github.com/droptype/proto) is a front-end web prototyping tool, combining markup ([Jade](http://jade-lang.com/)), script ([CoffeeScript](http://coffeescript.org)), and style ([Stylus](http://learnboost.github.com/stylus/)) into a single page. It creates a set of files each representing one of those three facets of the page, plus files for notes and settings, and serves up their rendered form. Every time the page is loaded, those files are compiled on-the-fly. It's helpful for creating prototypes using CoffeeScript, Jade, and Stylus, without having to set up a build process and environment.

* [Repository](https://github.com/droptype/proto)
* [Issues](https://github.com/droptype/proto/issues)
* [Example Proto project as Gist](https://gist.github.com/3894924)
* [Motivation & walkthrough](http://marquee.by/alecperkins/proto/)



## Installation

Proto is a command-line tool built in [Node](http://nodejs.org/), specifically [CoffeeScript](http://coffeescript.org), and is available through [npm](https://npmjs.org/).

    $ npm install -g proto-cli

or from source

    $ git clone git://github.com/droptype/proto.git
    $ cd proto
    $ mkdir lib
    $ cake build
    $ npm install -g .

`cake build` will compile `src/*.coffee` into `lib/*.js` (ignored by git).

Proto needs to be installed globally using `-g` so it can create the necessary command in `/usr/local/bin`.



## Usage

### Init

    $ proto -i <project_name>
    $ proto -ig <gist_id_or_url> [<project_name>]

Initializes the project by creating a folder with the specified name and adding five files: `markup.jade`, `script.coffee`, `style.styl`, `settings.json`, and `notes.md`.

e.g. `$ proto -i my_project` creates a folder called `my_project` in the current working directory

    my_project/
        markup.jade       - the source for the markup code
        script.coffee     - the source for the script code
        style.styl        - the source for the style code
        settings.json     - settings for the project, specifically extra libraries to include into the page
        notes.md          - a place for extra notes

You can also load a Gisted project (see below for how to create one). `proto -ig <gist_id_or_url> [<name>]` will initialize a Proto project using the specified gist as the template, with the name specified in its `settings.json` or the optional specified name.


### Work on a project

To serve the project at [localhost:5000](http://localhost:5000):

    $ proto <project_name>

Or specify a port:

    $ proto <project_name> -p 8080

This starts a server that serves the compiled markup, script, and style on the specified port (default 5000). The source files are compiled every time the page is requested.

The source files are compiled and inserted into a full `html` template. Libraries specified in `settings.json`, and the CSS compiled from `style.styl`, are added to the `<head>` of the page. `markup.jade` gets compiled to HTML and inserted into the `<body>`, and `script.coffee` gets compiled to JavaScript and added to the end of the `<body>`. (Take a peak at the Proto source for the [full template](https://github.com/droptype/proto/blob/master/src/renderer.coffee#L41) it uses.)

To have additional libraries loaded, list them in `script_libraries` or `style_libraries`. They must be served from somewhere else, like a [CDN](http://cdnjs.com/). Any extra markup to be inserted into the `<head>`, like viewport-width `<meta>` tags, can be specified in `extra_head_markup`.


### Gist a project

To create a GitHub [Gist](https://gist.github.com) with the project's contents:

    $ proto -g <project_name>
    $ proto -g <project_name> --public

This will upload the five files in the specified project folder to an anonymous Gist. By default, the Gist is private. Adding the `--public` flag will make it a public Gist. But, anonymous Gists aren't terribly useful besides one-off sharing, so *Authenticated* Gists are recommended.

#### Authenticated

To create the Gist under your username, first authenticate with GitHub using:

    $ proto --github <username> <password>
    $ proto --github <username> "<password with spaces>"

This will use the GitHub API to [generate an access token](http://developer.github.com/v3/oauth/#create-a-new-authorization) that is stored in `~/.proto-cli`. Your username and password are *never* stored.

Now, all Gists you create will be associated with your account. This has several benefits, including making the Proto project a git repo with the remote set to the Gist, so you can keep updating the Proto's Gist. Using `proto -g <project_name>` on a project that has already been Gisted with authentication will commit and push your changes to the same Gist, instead of creating a new one.



### Viewer

Proto includes a web viewer running at [proto.es](http://proto.es), which will render the specified Gist the same as the command-line would. It allows for easy sharing of the rendered Proto project, even specific versions of it (since it's just a git repo).

Example with the [sample project](https://gist.github.com/3894924): [proto.es/3894924](http://proto.es/3894924) ([older version](http://proto.es/3894924/e7496f08d10dce02db7209473912a1aa0676ce13))

The viewer is running on Heroku, with an alternate url that also works: [proto-cli.herokuapp.com](http://proto-cli.herokuapp.com). You can run your own viewer if you like. It's a Node-based web app located in [viewer/app.coffee](https://github.com/droptype/proto/blob/master/viewer/app.coffee).

Note: [Droptype](http://github.com/droptype) does have analytics code on the viewer, so we can monitor usage and make improvements. But, this code is only added to the page if the Gist is public, and only tracks from the `proto.es` and `proto-cli.herokuapp.com` domains. See exactly what it does in the [source](https://github.com/droptype/proto/blob/master/viewer/app.coffee).



## FAQ

### Why not LiveReload?

[LiveReload](http://livereload.com/) is awesome and works great — in fact it works really well alongside Proto — but doesn't serve the files (and nor should it). Certain JavaScript features require the file to be served instead of loaded using `file://` for security reasons. Proto is simpler to use and provides an easy way to initialize the project. It is also intended to be opinionated about the languages and structure it supports, creating simplicity through useful defaults.

### Why can't I have (more/fewer/other) files?

Convention. Proto restricts the sources to one file for each type to limit the kinds of things that can be built with it. It's a tool for prototyping relatively small interactions, kind of like a command-line version of [Pad Hacker](http://padhacker.net) or [JSFiddle](http://jsfiddle.net). Keeping the projects simple also makes it easy for others to understand quickly.



## Authors

* [Alec Perkins](http://github.com/alecperkins) ([Droptype Inc](http://droptype.com))



## License

Unlicensed aka Public Domain. See [/UNLICENSE](https://github.com/droptype/proto/blob/master/UNLICENSE) for more information.


