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

## 3. Validación taxonómica, distribución y criterios de selección
**Fecha de consulta:** 11 de febrero de 2026
**Base de datos:** ChiloBase 2.0: Lista de especies válidas del género Scolopendra consultada el  ([https://chilobase.biologia.unipd.it/searches/result_genres/117]

**Curación taxonómica y biogeográfica:** A partir de la consulta en ChiloBase, se compiló manualmente una tabla maestra (chilobase_especies_validas_distribucion.xlsx) que integra las especies nominales válidas y su información de distribución geográfica.

**Criterios de filtrado molecular:** Con base en el conteo de secuencias (grep -c ">") y la cobertura taxonómica por marcador, se evaluó la representatividad de los marcadores recuperados (descartando 28S tras el análisis de disponibilidad/cobertura) y se consolidaron los conjuntos curados finales en data/fasta/, depurando duplicados y verificando la validez nomenclatural de cada taxón.

**Trazabilidad:** El listado íntegro de números de acceso y encabezados curados incluidos en el estudio se encuentra registrado en data/metadata/inventario_secuencias_curadas.txt

## 4. Curación de secuencias y flujo bioinformático
* **Depuración taxonómica:** Se excluyeron secuencias sin determinación a nivel de especie (incluyendo registros indeterminados tipo *"Scolopendra* sp."), depurando duplicados y fragmentos de baja calidad.
* **Alineamiento de secuencias:** Los conjuntos filtrados por marcador se alinearon de forma independiente, verificando la dirección de las secuencias y revisando zonas con inserciones o deleciones anómalas.
* **Limpieza del alineamiento:** Se aplicó recorte automatizado de columnas para eliminar posiciones con alta proporción de huecos o alineamiento ambiguo antes del análisis filogenético.
* **Reconstrucción filogenética:** Las matrices recortadas se emplearon para inferir árboles filogenéticos bajo el modelo de sustitución óptimo por marcador/partición y evaluar el soporte nodal de los clados.
