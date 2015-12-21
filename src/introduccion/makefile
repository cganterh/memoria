sources = $(wildcard *.md)
fig_path = fig

.PHONY: all clean

all: $(sources:.md=.tex)

%.tex: %.md
	pandoc $(global_pandoc_opt) -o $@ $<

$(fig_path)/%.pdf: $(fig_path)/%.svg
	inkscape $< --export-pdf=$@

introduccion.tex: $(fig_path)/mapa.pdf

clean:
	rm -f *.tex
