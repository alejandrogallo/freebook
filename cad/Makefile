SOURCES_DIR ?= $(wildcard *.dir)
SOURCES_ZIP ?= $(patsubst %.dir,%.FCStd,$(SOURCES_DIR))
UNZIP       ?= unzip
ZIP         ?= zip
YES         ?= yes
DEPS_DIR    ?= .deps
ZIP_FILES_NAME ?= zip-files.txt
FILES_ZIP   ?= $(patsubst %.dir,%.dir/$(ZIP_FILES_NAME),$(SOURCES_DIR))

.DEFAULT_GOAL := all

all: $(SOURCES_DIR) $(FILES_ZIP) ## Uncompress FCStd files into directories to update

freecad-files: $(SOURCES_ZIP) ## Compress directories into freecad files

%.dir: %.FCStd
	$(YES) | $(UNZIP) $< -d $@

%.FCStd: %.dir/ %.dir/$(ZIP_FILES_NAME)
	$(info Creating $@ from $< )
	cd $*.dir; $(ZIP) ../$@ -@ < $(ZIP_FILES_NAME)

%.dir/$(ZIP_FILES_NAME): %.FCStd
	$(UNZIP) -l $*.FCStd \
		| sed -e "1,4d; " \
		| tac \
		| sed 1,2d \
		| tac \
		| awk '{print $$4}' > $@

clean:
	rm $(SOURCES_ZIP)


# This is used for printing defined variables from Some other scripts. For
# instance if you want to know the value of the PDF_VIEWER defined in the
# Makefile, then you would do
#    make print-PDF_VIEWER
# and this would output PDF_VIEWER=mupdf for instance.
FORCE:
print-%:
	@echo '$*=$($*)'

help: ## Prints help for targets with comments
	@$(or $(AWK),awk) ' \
		BEGIN {FS = ":.*?## "}; \
		/^## *<<HELP/,/^## *HELP/ { \
			help=$$1; \
			gsub("#","",help); \
			if (! match(help, "HELP")) \
				print help ; \
		}; \
		/^[a-zA-Z0-9_\-.]+:.*?## .*$$/{ \
			printf "\033[36m%-30s\033[0m %s\n", $$1, $$2 ; \
		};' \
		$(MAKEFILE_LIST)


# vim: nospell fdm=marker cc=80
