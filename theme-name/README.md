Bootstrap-Overrides (optional reading)
-----------------------------------------

The purpose of this boilerplate is to make your own customizations to bootstrap without touching the core.

These are the techniques I use to do so. The process to get to this point is the following:

1. Copy ```bootstrap.less```, ```responsive.less```, and ```variables.less``` into a mirrored directory structure called ```bootstrap-overrides```.

        - bootstrap-overrides
          - less

2. Copy ```package.json``` to ```bootstrap-overrides/```

3. Copy ```Makefile```

Next I modified a few things in ```bootstrap-overrides``` to get our build process to work using our overrides.

1. Modify ```bootstrap.less```, and ```responsive.less``` to import from the bootstrap submodule for everything except ```variables.less```

2. Add a variable called ```BOOTSTRAP_DIR``` to the ```Makefile```

       ```BOOTSTRAP_DIR = ../bootstrap```

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

5. Add some node_modules as dev dependencies, and reference them from script, like watchr
