BOOTSTRAP_DIR = ../bootstrap
BOOTSTRAP = ${BOOTSTRAP_DIR}/docs/assets/css/bootstrap.css
BOOTSTRAP_LESS = ./less/bootstrap.less
BOOTSTRAP_RESPONSIVE = ${BOOTSTRAP_DIR}/docs/assets/css/bootstrap-responsive.css
BOOTSTRAP_RESPONSIVE_LESS = ./less/responsive.less
DATE=$(shell date +%I:%M%p)
CHECK=\033[32m✔\033[39m
HR=\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#


#
# BUILD DOCS
#

build:
	@echo "\n${HR}"
	@echo "Building Bootstrap..."
	@echo "${HR}\n"
	@./node_modules/.bin/jshint ${BOOTSTRAP_DIR}/js/*.js --config ${BOOTSTRAP_DIR}/js/.jshintrc
	@./node_modules/.bin/jshint ${BOOTSTRAP_DIR}/js/tests/unit/*.js --config ${BOOTSTRAP_DIR}/js/.jshintrc
	@echo "Running JSHint on javascript...             ${CHECK} Done"
	@./node_modules/.bin/recess --compile ${BOOTSTRAP_LESS} > ${BOOTSTRAP}
	@./node_modules/.bin/recess --compile ${BOOTSTRAP_RESPONSIVE_LESS} > ${BOOTSTRAP_RESPONSIVE}
	@echo "Compiling LESS with Recess...               ${CHECK} Done"
	@node ${BOOTSTRAP_DIR}/docs/build
	@cp ${BOOTSTRAP_DIR}/img/* ${BOOTSTRAP_DIR}/docs/assets/img/
	@cp ${BOOTSTRAP_DIR}/js/*.js ${BOOTSTRAP_DIR}/docs/assets/js/
	@cp ${BOOTSTRAP_DIR}/js/tests/vendor/jquery.js ${BOOTSTRAP_DIR}/docs/assets/js/
	@echo "Compiling documentation...                  ${CHECK} Done"
	@cat ${BOOTSTRAP_DIR}/js/bootstrap-transition.js ${BOOTSTRAP_DIR}/js/bootstrap-alert.js ${BOOTSTRAP_DIR}/js/bootstrap-button.js ${BOOTSTRAP_DIR}/js/bootstrap-carousel.js ${BOOTSTRAP_DIR}/js/bootstrap-collapse.js ${BOOTSTRAP_DIR}/js/bootstrap-dropdown.js ${BOOTSTRAP_DIR}/js/bootstrap-modal.js ${BOOTSTRAP_DIR}/js/bootstrap-tooltip.js ${BOOTSTRAP_DIR}/js/bootstrap-popover.js ${BOOTSTRAP_DIR}/js/bootstrap-scrollspy.js ${BOOTSTRAP_DIR}/js/bootstrap-tab.js ${BOOTSTRAP_DIR}/js/bootstrap-typeahead.js ${BOOTSTRAP_DIR}/js/bootstrap-affix.js > ${BOOTSTRAP_DIR}/docs/assets/js/bootstrap.js
	@./node_modules/.bin/uglifyjs -nc ${BOOTSTRAP_DIR}/docs/assets/js/bootstrap.js > ${BOOTSTRAP_DIR}/docs/assets/js/bootstrap.min.tmp.js
	@echo "/**\n* Bootstrap.js v2.3.1 by @fat & @mdo\n* Copyright 2012 Twitter, Inc.\n* http://www.apache.org/licenses/LICENSE-2.0.txt\n*/" > ${BOOTSTRAP_DIR}/docs/assets/js/copyright.js
	@cat ${BOOTSTRAP_DIR}/docs/assets/js/copyright.js ${BOOTSTRAP_DIR}/docs/assets/js/bootstrap.min.tmp.js > ${BOOTSTRAP_DIR}/docs/assets/js/bootstrap.min.js
	@rm ${BOOTSTRAP_DIR}/docs/assets/js/copyright.js ${BOOTSTRAP_DIR}/docs/assets/js/bootstrap.min.tmp.js
	@echo "Compiling and minifying javascript...       ${CHECK} Done"
	@echo "\n${HR}"
	@echo "Bootstrap successfully built at ${DATE}."
	@echo "${HR}\n"
	@echo "Thanks for using Bootstrap,"
	@echo "<3 @mdo and @fat\n"

#
# RUN JSHINT & QUNIT TESTS IN PHANTOMJS
#

test:
	./node_modules/.bin/jshint ${BOOTSTRAP_DIR}/js/*.js --config ${BOOTSTRAP_DIR}/js/.jshintrc
	./node_modules/.bin/jshint ${BOOTSTRAP_DIR}/js/tests/unit/*.js --config ${BOOTSTRAP_DIR}/js/.jshintrc
	node ${BOOTSTRAP_DIR}/js/tests/server.js &
	./node_modules/.bin/phantomjs ${BOOTSTRAP_DIR}/js/tests/phantom.js "http://localhost:3000/js/tests"
	kill -9 `cat ${BOOTSTRAP_DIR}/js/tests/pid.txt`
	rm ${BOOTSTRAP_DIR}/js/tests/pid.txt

#
# CLEANS THE ROOT DIRECTORY OF PRIOR BUILDS
#

clean:
	rm -r bootstrap

#
# BUILD SIMPLE BOOTSTRAP DIRECTORY
# recess & uglifyjs are required
#

bootstrap: bootstrap-img bootstrap-css bootstrap-js


#
# JS COMPILE
#
bootstrap-js: bootstrap/js/*.js

bootstrap/js/*.js: js/*.js
	mkdir -p bootstrap/js
	cat js/bootstrap-transition.js js/bootstrap-alert.js js/bootstrap-button.js js/bootstrap-carousel.js js/bootstrap-collapse.js js/bootstrap-dropdown.js js/bootstrap-modal.js js/bootstrap-tooltip.js js/bootstrap-popover.js js/bootstrap-scrollspy.js js/bootstrap-tab.js js/bootstrap-typeahead.js js/bootstrap-affix.js > bootstrap/js/bootstrap.js
	./node_modules/.bin/uglifyjs -nc bootstrap/js/bootstrap.js > bootstrap/js/bootstrap.min.tmp.js
	echo "/*!\n* Bootstrap.js by @fat & @mdo\n* Copyright 2012 Twitter, Inc.\n* http://www.apache.org/licenses/LICENSE-2.0.txt\n*/" > bootstrap/js/copyright.js
	cat bootstrap/js/copyright.js bootstrap/js/bootstrap.min.tmp.js > bootstrap/js/bootstrap.min.js
	rm bootstrap/js/copyright.js bootstrap/js/bootstrap.min.tmp.js

#
# CSS COMPLILE
#

bootstrap-css: bootstrap/css/*.css

bootstrap/css/*.css: less/*.less
	mkdir -p bootstrap/css
	./node_modules/.bin/recess --compile ${BOOTSTRAP_LESS} > bootstrap/css/bootstrap.css
	./node_modules/.bin/recess --compress ${BOOTSTRAP_LESS} > bootstrap/css/bootstrap.min.css
	./node_modules/.bin/recess --compile ${BOOTSTRAP_RESPONSIVE_LESS} > bootstrap/css/bootstrap-responsive.css
	./node_modules/.bin/recess --compress ${BOOTSTRAP_RESPONSIVE_LESS} > bootstrap/css/bootstrap-responsive.min.css

#
# IMAGES
#

bootstrap-img: bootstrap/img/*

bootstrap/img/*: img/*
	mkdir -p bootstrap/img
	cp img/* bootstrap/img/


#
# MAKE FOR GH-PAGES 4 FAT & MDO ONLY (O_O  )
#

gh-pages: bootstrap docs
	rm -f docs/assets/bootstrap.zip
	zip -r docs/assets/bootstrap.zip bootstrap
	rm -r bootstrap
	rm -f ../bootstrap-gh-pages/assets/bootstrap.zip
	node docs/build production
	cp -r docs/* ../bootstrap-gh-pages

#
# WATCH LESS FILES
#

watch:
	echo "Watching less files..."; \
	./node_modules/.bin/watchr -e "watch('less/.*\.less') { system 'make' }"


.PHONY: docs watch gh-pages bootstrap-img bootstrap-css bootstrap-js