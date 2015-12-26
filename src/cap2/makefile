sources = $(wildcard *.md)
fig_path = fig

.PHONY: all clean

all: $(sources:.md=.tex)

%.tex: %.md
	pandoc $(global_pandoc_opt) -o $@ $<

$(fig_path)/%.pdf: $(fig_path)/%.svg
	inkscape $< --export-pdf=$@

cap2.tex: $(fig_path)/stack.pdf $(fig_path)/arquitectura.pdf

clean:
	rm -f *.tex
