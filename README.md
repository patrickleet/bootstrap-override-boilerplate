bootstrap-override-boilerplate
==============================

A boilerplate for easy and unobtrusive overriding of bootstrap with a custom theme.
Allows for unobtrusive overriding of all bootstrap less files, docs, and a build process.

How to Use
==========

1. Bootstrap Submodule
2. Bootstrap Overrides
3. Making Customizations

1) Bootstrap Submodule
----------------------

I've found it's optimal to include bootstrap as a submodule to easily keep it up to date with the latest version of Twitter Bootstrap.

Follow these instructions for cloning a project with submodules:
[Cloning a Project with Submodules](http://git-scm.com/book/en/Git-Tools-Submodules#Cloning-a-Project-with-Submodules)

tl;dr;:

    git clone <<git-url>>
    git submodule init
    git submodule update

2) Bootstrap-Overrides
----------------------

The purpose of this boilerplate is to make your own customizations to bootstrap without touching the core.

These are the techniques I use to do so. The process to get to this point is the following:

1. Copy ```bootstrap.less```, ```responsive.less```, and ```variables.less``` into a mirrored directory structure called ```bootstrap-overrides```.

        - bootstrap-overrides
          - less

2. Copy ```package.json``` to ```bootstrap-overrides/```

3. Copy ```Makefile```

Next we need to modify a few things in our ```bootstrap-overrides``` to get our build process to work using our overrides.

1. Modify ```bootstrap.less```, and ```responsive.less``` to import from the bootstrap submodule for everything except ```variables.less```

2. Add a variable called ```BOOTSTRAP_DIR``` to the ```Makefile```

       BOOTSTRAP_DIR = ../bootstrap

3. Prefix all referenced dirs in the build and test section with ```${BOOTSTRAP_DIR}/```

        build:
          @echo "\n${HR}"
          @echo "Building Bootstrap..."
          @echo "${HR}\n"
          @./node_modules/.bin/jshint ${BOOTSTRAP_DIR}/js/*.js --config ${BOOTSTRAP_DIR}/js/.jshintrc
          @./node_modules/.bin/jshint ${BOOTSTRAP_DIR}/js/tests/unit/*.js --config ${BOOTSTRAP_DIR}/js/.jshintrc
          etc..

4. Update constant locations of ```BOOTSTRAP``` and ```BOOTSTRAP_RESPONSIVE``` to the ```BOOTSTRAP_DIR```

        BOOTSTRAP_DIR = ../bootstrap
        BOOTSTRAP = ${BOOTSTRAP_DIR}/docs/assets/css/bootstrap.css
        BOOTSTRAP_LESS = ./less/bootstrap.less
        BOOTSTRAP_RESPONSIVE = ${BOOTSTRAP_DIR}/docs/assets/css/bootstrap-responsive.css
        BOOTSTRAP_RESPONSIVE_LESS = ./less/responsive.less

3) Making Customizations
------------------------

1. Find the code you want to override, for example, if you want to make all tables have a solid red border, you would want to override ```tables.less```

2. Create ```bootstrap-overrides/less/tables-overrides.less```

3. Import your new file directly after the corresponding import

        @import "../../bootstrap/less/tables.less";
        @import "tables-overrides.less";

4. Write less to override table styles in ```tables-overrides.less```


