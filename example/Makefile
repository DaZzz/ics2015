# original Makefile by Pasi "okun" Valminen, 2004
# edited by Pekka "PQ" Paalanen, 2004 - 2010
# edited bu Jukka "lanxu" Lankinen 2012 (Modified parameters)
# version 28.2.2012 for MSc thesis

## Modify these to your needs.

# document target basename
TARGET   := thesis

# Autogenerated source files, like pdf-figures created with
# gnuplot scripts. Target 'distclean' will delete files listed here.
AUTOTGT  := luxeonspectra.pdf


## The following should be ok as is.

# document source files
SRC      := $(wildcard *.tex *.bib *.cls *.sty) $(AUTOTGT)

# programs
SHELL    := /bin/bash
GS       := gs
TEXI2PDF := texi2pdf
GNUPLOT  := gnuplot
EPS2PDF  := epstopdf
#PDFVIEW  := xpdf -remote view-$(abspath $(TARGET).pdf)
PDFVIEW  := evince

GSPDF_OPTS := -sDEVICE=pdfwrite -dPDFSETTINGS=/printer \
              -dCompatibilityLevel=1.4 -dMaxSubsetPct=100 -dSubsetFonts=true \
              -dEmbedAllFonts=true

# The GS options are from http://www.ctan.org/tex-archive/macros/latex/
#   contrib/IEEEtran/testflow/testflow_doc.txt

# Texi2pdf runs bibtex automatically and pdflatex as many times as needed.

all: $(TARGET).pdf

clean:
	rm -f *.{aux,bbl,blg,tmp,dvi,toc,log,out,idx,ilg,ind,brf} *~

distclean: clean
	rm -f $(TARGET).pdf spread.pdf $(AUTOTGT)

view: $(TARGET).pdf
	$(PDFVIEW) $(TARGET).pdf &

# For printing a twosided draft, one spread per page.
spread.pdf: $(TARGET).pdf
	pdftops $(TARGET).pdf - | psselect _,- | psnup -d -2 | ps2pdf - spread.pdf

%.pdf: %.tex $(SRC)
	$(TEXI2PDF) $<

%.eps: %.gp
	cd $(dir $<) && $(GNUPLOT) $(notdir $<)

%.pdf: %.eps
	$(EPS2PDF) --nogs $< | $(GS) -q $(GSPDF_OPTS) \
	  -dAutoRotatePages=/None -sOutputFile=$@ - -c quit

.SUFFIXES:
.PRECIOUS: %.dvi %.pdf
.DELETE_ON_ERROR: %.dvi %.pdf
.PHONY: all clean distclean view
