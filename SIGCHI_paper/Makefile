all: paper.pdf

# Temporary folders where all latex voodoo takes place
TMP=tmp

# Special headers for windows
ifeq ($(OS),Windows_NT)
	BEFORE_BODY=--include-before-body=pandoc/latex-before_body.tex --include-before-body=pandoc/latex-before_body_win.tex
else
	BEFORE_BODY=--include-before-body=pandoc/latex-before_body.tex
endif

#FIXME: bibliography param not generic
%.pdf: %.md
	scholdoc $^ -t latex  \
	--include-in-header=pandoc/latex-headers_extra.tex $(BEFORE_BODY) --template pandoc/scholmdTemplate_sigchi.latex \
	-V documentclass:pandoc/sigchi \
	-V biblio-style=pandoc/SIGCHI-Reference-Format --csl=pandoc/acm-sigchi-proceedings.csl --bibliography=biblio.bib --natbib \
	-o $(TMP)/$*.tex
	# generate .aux
	pdflatex -draftmode -output-directory $(TMP) $(TMP)/$*
	# make bib refs
	bibtex $(TMP)/$*
	# update refs
	pdflatex -draftmode -output-directory $(TMP) $(TMP)/$*
	# actually build the pdf
	pdflatex -output-directory $(TMP) $(TMP)/$*
	# bring forth the file
	cp $(TMP)/$@ ./

%.docx: %.md
	scholdoc $^ -o $@ --csl=pandoc/acm-sigchi-proceedings.csl --bibliography=biblio.bib

#NB: scholdoc doesn't do odt...
%.odt: %.md
	pandoc $^ -o $@ --csl=pandoc/acm-sigchi-proceedings.csl --bibliography=biblio.bib

clean:
	rm -rf *.pdf *.docx *.odt tmp/*

.PHONY: all clean
