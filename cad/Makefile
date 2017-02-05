SOURCES ?= $(wildcard *.FCStd)
TARGETS ?= $(patsubst %.FCStd,%.dir,$(SOURCES))
UNZIP   ?= unzip
ZIP     ?= zip
YES     ?= yes

.DEFAULT_GOAL := all

all: $(TARGETS) ## Uncompress FCStd files into directories

%.dir: %.FCStd
	$(YES) | $(UNZIP) $< -d $@

%.FCStd: %.dir
	$(ZIP) -r -j $@ $<


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
