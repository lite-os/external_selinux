CWD ?= $(shell pwd)
HTMLDIR ?= $(CWD)/html
PDFDIR ?= $(CWD)/pdf
TMPDIR ?= $(CWD)/tmp
TESTDIR ?= $(CWD)/../test

# All the markdown files that make up the guide:
FILE_LIST ?= cil_introduction.md \
	cil_reference_guide.md \
	cil_access_vector_rules.md \
	cil_call_macro_statements.md \
	cil_class_and_permission_statements.md \
	cil_conditional_statements.md \
	cil_constraint_statements.md \
	cil_container_statements.md \
	cil_context_statement.md \
	cil_default_object_statements.md \
	cil_file_labeling_statements.md \
	cil_mls_labeling_statements.md \
	cil_network_labeling_statements.md \
	cil_policy_config_statements.md \
	cil_role_statements.md \
	cil_sid_statements.md \
	cil_type_statements.md \
	cil_user_statements.md \
	cil_xen_statements.md

PANDOC_FILE_LIST = $(addprefix $(TMPDIR)/,$(FILE_LIST))

PDF_OUT=CIL_Reference_Guide.pdf
HTML_OUT=CIL_Reference_Guide.html
PANDOC = pandoc

all: html pdf

$(TMPDIR):
	mkdir -p $(TMPDIR)

$(TMPDIR)/%.md: %.md | $(TMPDIR)
	cp -f $< $(TMPDIR)/
	@# Substitute markdown links for conversion into PDF links
	sed -i -re 's:(\[`[^`]*`\])\([^#]*([^\)]):\1\(\2:g' $@

$(TMPDIR)/policy.cil: $(TESTDIR)/policy.cil
	cp -f $< $@
	@# add a title for the TOC to policy.cil. This is needed to play nicely with the PDF conversion.
	sed -i '1i Example Policy\n=========\n```' $@
	echo '```' >> $@

html: $(PANDOC_FILE_LIST) $(TMPDIR)/policy.cil
	mkdir -p $(HTMLDIR)
	$(PANDOC) -t html $^ -o $(HTMLDIR)/$(HTML_OUT)

pdf: $(PANDOC_FILE_LIST) $(TMPDIR)/policy.cil
	mkdir -p $(PDFDIR)
	$(PANDOC) --standalone --toc $^ -o $(PDFDIR)/$(PDF_OUT)

clean:
	rm -rf $(HTMLDIR)
	rm -rf $(PDFDIR)
	rm -rf $(TMPDIR)
