test_bundle := build/test.js
node_modules := node_modules/.bin/mocha

all: $(test_bundle)

$(node_modules):
	npm install

core:
	cd ../core; ./gradlew --quiet jsMainClasses jsTestClasses
	cp ../core/build/classes/kotlin/js/*/*.js node_modules/
	mkdir -p build/assets
	rsync -a ../core/assets/main/ build/assets/
	rsync -a ../core/assets/test/ build/assets/

$(test_bundle): src/test/index.js core
	mkdir -p build/lib build/test build/css
	npx webpack -d --mode development --target web --output $@ $<
	cp src/test/*html build/test
	cp node_modules/mocha/mocha.css build/lib
	cp node_modules/mocha/mocha.js build/lib
	cp node_modules/sql.js/js/sql.js build/lib

serve:
	npx serve build/

clean:
	rm -rf build

distclean: clean
	rm -rf node_modules

.PHONY: test clean distclean core
