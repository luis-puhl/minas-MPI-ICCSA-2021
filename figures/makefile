
all: lifecycle.png mfog-arch-fisica.pdf

# inkscape -D $? -o $@ --export-latex
%.pdf: %.svg
	rsvg-convert -f pdf -o $@ $?

%.svg: %.puml
	plantuml -tsvg -pipe < $? > $@

# plantuml -tpng -pipe < $? > $@
%.png: %.svg
	inkscape $? --export-filename=$@ --export-dpi=600 --export-background="#ffffffff"
