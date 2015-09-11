# Makefile for DocBook projects: Basic version
#================================================================
# Definitions
#----------------------------------------------------------------
# BASENAME:        The base file name of the Asciidoc/docbook file.
# SOURCE:          The .xml file containing the document.
# WEB_TARGET:      Top-level generated HTML page name.
# FO_TARGET:       Intermediate file for generated PDF.
# PDF_TARGET:      Generated PDF file name.
# INDEX_BASE:      Base name for index of section id values.
# INDEX_FO:        Intermediate file for section index.
# INDEX_PDF:       Section index PDF file name.
# XSL_DIR:         Where the local customization layer lives.
# HTML_TRANSFORM:  XSLT to build the HTML rendering.
# PDF_TRANSFORM:   XSLT to build the PDF rendering.
# XSLT_HTML_OPT passes options to xsltproc to build the HTML form.
#     To push chunking down to subsections:
#        --stringparam chunk.section.depth 2 
#     To move the first subsection to its own chunk:
#        --stringparam chunk.first.sections 1
#     To show subsubsections in the table of contents:
#        --stringparam toc.section.depth 3
# XSLT_FO_OPT:  Options for hardcopy.
#     To show subsubsections in the table of contents:
#        --stringparam toc.section.depth 3
# FOP_OPT:  Options to fop
#----------------------------------------------------------------
BASENAME        =  main
SOURCE          =  $(BASENAME).asciidoc
WEB_TARGET      =  index.html
FO_TARGET       =  $(BASENAME).fo
PDF_TARGET      =  $(BASENAME).pdf
INDEX_BASE      =  toc
INDEX_FO        =  $(INDEX_BASE).fo
INDEX_PDF       =  $(INDEX_BASE).pdf
XSL_DIR         =  ./styles/
HTML_TRANSFORM  =  $(XSL_DIR)lit_html.xsl
FO_TRANSFORM    =  $(XSL_DIR)lit_fo.xsl
XSLT_HTML_OPT   =  --stringparam chunk.section.depth 2 \
                   --stringparam chunk.first.sections 1
XSLT_FO_OPT     =  
FOP_OPT         =  

#--
# Add the suffixes that we are interested in
#--
# .fo       XSL-FO output
# .html     HTML web pages                 
# .pdf      Page Description Format (Abode)
# .xml      Input files in DocBook XML format
#--
.SUFFIXES: .asciidoc .fo .html .pdf .xml

#================================================================
# Default suffix rules
#----------------------------------------------------------------
.asciidoc.xml:
	asciidoc -b docbook -o $@ $<
.xml.html:
	xsltproc -o $@ $(XSLT_HTML_OPT) $(HTML_TRANSFORM) $<

.xml.fo:
	xsltproc -o $@ $(XSLT_FO_OPT) $(FO_TRANSFORM) $<

.fo.pdf:
	fop $< $@ 

#================================================================
# Targets
#----------------------------------------------------------------
all: pdf

web: $(WEB_TARGET)

$(WEB_TARGET): $(SOURCE)
	xsltproc -o $@ $(XSLT_HTML_OPT) $(HTML_TRANSFORM) $<

pdf: $(PDF_TARGET)

$(PDF_TARGET): $(SOURCE)

index: $(INDEX_PDF)

$(INDEX_PDF): $(INDEX_FO)

$(INDEX_FO): $(SOURCE)
	rm -f $(INDEX_FO); \
	docbookindex $(SOURCE)

clean:
	rm -f *.xml *.fo *.html *.pdf
