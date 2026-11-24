# Registro de consultas, separación por marcador y curación de datos moleculares

## 1. Recuperación de secuencias en NCBI Nucleotide
* **Periodo de consulta:** Febrero de 2026 (inicio de etapa de recopilación de datos).
* **Base de datos:** `nuccore` (NCBI Nucleotide).
* **Comando ejecutado (Entrez Direct):**
  ```bash
  esearch -db nuccore -query "Scolopendra[Organism] AND (16S OR 18S OR 28S OR COI)" | \
  esummary | xtract -pattern DocumentSummary -element Caption,TaxId,Title > lista_secuencias.txt

## 2. Extracción de secuencias por marcador
for marker in "16S" "18S" "28S" "COI" "CO1"; do
    grep -A 1 -i "$marker" scolopendra_markers.fasta | grep -v "^--$" > "Scolopendra_$marker.fasta"
done

# Conteo de secuencias recuperadas por marcador
grep -c ">" Scolopendra_*.fasta
