GLOBAL_DEPS=Makefile $(wildcard src/**.es6) build/build.js build/config.js node_modules

PROJECT_DIR=$(patsubst %/,%,$(dir $(abspath $(lastword $(MAKEFILE_LIST)))))
GH_PAGES_PATH=$(PROJECT_DIR)/gh-pages

.PHONY: all
all: test build

.PHONY: test
test: $(GLOBAL_DEPS)
	yarn test

.PHONY: build
build: $(GLOBAL_DEPS)
	yarn build

node_modules:
	yarn install --frozen-lockfile

.PHONY: release-docs
release-docs: gh-pages build $(PROJECT_DIR)/dist/samjs.min.js $(PROJECT_DIR)/dist/guessnum.min.js
	cp $(PROJECT_DIR)/dist/samjs.min.js \
		$(PROJECT_DIR)/dist/guessnum.min.js \
		$(GH_PAGES_PATH)/dist
	git -C $(GH_PAGES_PATH) add .
	git -C $(GH_PAGES_PATH) commit -m "update release"

.PHONY: gh-pages
gh-pages: $(GLOBAL_DEPS)
ifeq ($(wildcard $(GH_PAGES_PATH)/),)
	git clone -b gh-pages $(shell git remote get-url --push origin) $(GH_PAGES_PATH)
else
	(cd $(GH_PAGES_PATH) && git pull)
endif

UID="$(shell id -u)"
GID="$(shell id -g)"
PROJECT_DIR=$(realpath $(dir $(abspath $(lastword $(MAKEFILE_LIST)))))
.PHONY: build-docker-image
build-docker-image:
	docker build build/docker -t sam-build:latest

.PHONY: build-with-docker
build-with-docker: build-docker-image
	docker run --rm -it --user $(UID):$(GID) -v$(PROJECT_DIR):/project sam-build:latest make all

.PHONY: node-docker
node-shell: build-docker-image
	docker run --rm -it --user $(UID):$(GID) -v$(PROJECT_DIR):/project sam-build:latest /bin/sh
