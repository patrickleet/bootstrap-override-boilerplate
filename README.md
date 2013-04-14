bootstrap-override-boilerplate
==============================

A boilerplate for easy and unobtrusive overriding of bootstrap with a custom theme.
Allows for unobtrusive overriding of all bootstrap less files, docs, and a build process.

How to Use
==========

1. Bootstrap Submodule
2. Making Customizations
3. Building

Bootstrap Submodule
-------------------

I've found it's optimal to include bootstrap as a submodule to easily keep it up to date with the latest version of Twitter Bootstrap.

Follow these instructions for cloning a project with submodules:
[Cloning a Project with Submodules](http://git-scm.com/book/en/Git-Tools-Submodules#Cloning-a-Project-with-Submodules)

tl;dr;:

    git clone <<git-url>>
    cd <<dir>>
    git submodule init
    git submodule update


Making Customizations
---------------------

1. Find the code you want to override, for example, if you want to make all tables have a solid red border, you would want to override ```tables.less```

2. Create ```theme-template/less/tables-overrides.less```

3. Import your new file directly after the corresponding import

        @import "../../bootstrap/less/tables.less";
        @import "tables-overrides.less";

4. Write less to override table styles in ```tables-overrides.less```

Building
--------

Once you've made changes it's easy to build and see your changes applied against the bootstrap docs.

1. First start by changing to the ```theme-template``` dir

    ```cd theme-template```

2. Install all deps with ```npm install```
3. Build with ```make```

Now you can open ```bootstrap/docs/index.html``` and see your live style guide.

Optionally, you can watch files with ```make watch```

About
=====

Bootstrap-Overrides (optional reading)
-----------------------------------------

The purpose of this boilerplate is to make your own customizations to bootstrap without touching the core.

These are the techniques I use to do so. The process to get to this point is the following:

1. Copy everything to a mirrored directory structure of ```bootstrap``` named ```theme-template```.
2. In ```less```, delete everything except ```bootstrap.less```, ```responsive.less```, and ```variables.less```.
3. Modify ```bootstrap.less```, and ```responsive.less``` to import from the bootstrap submodule for everything except ```variables.less```
4. Add some node_modules as dev dependencies, and reference them from script, like watchr, so they don't need to be installed globally.
