# ============================================================
# Benchmark de Ball Pivoting multi-semilla con OpenMP
# Ejecutar desde 06_CPU_vs_CUDA
# ============================================================

CXX      ?= g++
CXXFLAGS ?= -O2 -std=c++17 -Wall -Wextra
OPENMP   ?= -fopenmp
LIBS     ?= -lglut -lGL -lGLU -lm

SRC      ?= 03_Stanford_Bunny_BPA_MULTISEMILLA_PARALELO.cpp
BIN      ?= bpa_paralelo

XYZ_2503  ?= ../05_BPA_Bunny_2503/Stanford_Bunny_2503.xyz
XYZ_20000 ?= Stanford_Bunny_20000.xyz
XYZ_50000 ?= Stanford_Bunny_50000.xyz

# 6 nucleos fisicos. Para probar los 12 hilos logicos:
#   make benchmark THREADS="1 2 4 6 12"
THREADS ?= 1 2 4 6
RADIO   ?= 0.007
OUTDIR  ?= resultados
TIMEOUT ?= 900

.PHONY: all parallel benchmark benchmark-50k clean help

all: $(BIN)

parallel: $(BIN)

$(BIN): $(SRC)
	$(CXX) $(CXXFLAGS) $(OPENMP) $< -o $@ $(LIBS)

$(OUTDIR):
	mkdir -p $(OUTDIR)

benchmark: $(BIN) | $(OUTDIR)
	@echo "dataset,hilos,radio,tiempo_ms,triangulos,regiones,puntos_cubiertos" \
		> $(OUTDIR)/resumen.csv
	@for spec in "2503:$(XYZ_2503)" "20000:$(XYZ_20000)"; do \
		name=$${spec%%:*}; \
		file=$${spec#*:}; \
		for t in $(THREADS); do \
			echo "== $$name puntos, $$t hilos =="; \
			OMP_NUM_THREADS=$$t ./$(BIN) --bench --csv --radio $(RADIO) "$$file" \
				> $(OUTDIR)/$${name}_$${t}hilos.txt; \
			line=$$(grep -E '^[0-9]+,[0-9]+,0\.' \
				$(OUTDIR)/$${name}_$${t}hilos.txt | tail -n 1); \
			if [ -n "$$line" ]; then \
				values=$$(echo "$$line" | cut -d, -f2-); \
				echo "$$name,$$values" >> $(OUTDIR)/resumen.csv; \
			else \
				echo "$$name,$$t,$(RADIO),error,,," \
					>> $(OUTDIR)/resumen.csv; \
			fi; \
		done; \
	done
	@echo "Resultados en $(OUTDIR)/resumen.csv"

benchmark-50k: $(BIN) | $(OUTDIR)
	@echo "dataset,hilos,radio,tiempo_ms,triangulos,regiones,puntos_cubiertos" \
		> $(OUTDIR)/resumen_50k.csv
	@for t in $(THREADS); do \
		echo "== 50000 puntos, $$t hilos =="; \
		OMP_NUM_THREADS=$$t timeout $(TIMEOUT) ./$(BIN) --bench --csv \
			--radio $(RADIO) "$(XYZ_50000)" \
			> $(OUTDIR)/50000_$${t}hilos.txt; \
		status=$$?; \
		if [ $$status -eq 124 ]; then \
			echo "50000,$$t,$(RADIO),timeout,,," \
				>> $(OUTDIR)/resumen_50k.csv; \
		else \
			line=$$(grep -E '^[0-9]+,[0-9]+,0\.' \
				$(OUTDIR)/50000_$${t}hilos.txt | tail -n 1); \
			if [ -n "$$line" ]; then \
				values=$$(echo "$$line" | cut -d, -f2-); \
				echo "50000,$$values" >> $(OUTDIR)/resumen_50k.csv; \
			else \
				echo "50000,$$t,$(RADIO),error,,," \
					>> $(OUTDIR)/resumen_50k.csv; \
			fi; \
		fi; \
	done
	@echo "Resultados en $(OUTDIR)/resumen_50k.csv"

clean:
	rm -f $(BIN)
	rm -rf $(OUTDIR)

help:
	@echo "make                  Compila la version paralela"
	@echo "make benchmark        Ejecuta 2503 y 20000 con 1,2,4,6 hilos"
	@echo "make benchmark-50k    Prueba de estres con 50000 puntos"
	@echo "make benchmark THREADS=\"1 2 4 6 12\"  Incluye hilos logicos"
	@echo "make clean            Borra el binario y los resultados"
