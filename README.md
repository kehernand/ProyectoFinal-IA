# Copiloto de movilidad – Valle de Aburrá (SI3003, EAFIT 2026-1)

A* multiobjetivo (tiempo, costo, emisiones) sobre un grafo multimodal + agente LLM con RAG y 2 herramientas (`plan_route`, `search_docs`) + dashboard Streamlit.

## Ejecutar
```bash
pip install -r requirements.txt
cp .env.example .env      # pegar LLM_API_KEY (Groq / NVIDIA NIM / Ollama)
streamlit run app.py
python eval/benchmark_search.py   # métrica Parte 1: A* vs Dijkstra vs BFS
```

## Estructura
- `core/search.py` A*, Dijkstra, BFS propios · `core/graph.py` grafo y supuestos · `core/rag.py` recuperador · `core/agent.py` loop agéntico (T_max=6)
- `corpus/` documentos del RAG (**reemplazar los [COMPLETAR] con texto oficial**)
- `eval/` benchmark del algoritmo (agregar `questions.json` con 10–15 preguntas del agente)

## Rutas por calles reales (opcional)
```bash
pip install -r requirements-osm.txt
python scripts/download_osm.py     # descarga la red vial (varios minutos, requiere internet)
```
Con `data/valle_drive.graphml` presente, el mapa sigue las calles; sin él, dibuja línea recta entre paradas.

## Datos de bus
`data/bus_routes.json` contiene corredores reales de bus del Valle de Aburrá (sur por La 50, Envigado–Poblado–centro, y norte por la Autopista Norte), con operador, tarifa 2026 y fuente. Las paradas se mapean a las estaciones de la Línea A que cada corredor sirve. Tarifa de abordaje del colectivo metropolitano 2026: $3.800 (general); ver fuentes en `corpus/metro_medellin.md`.

### Tarifas reales 2026 (SITVA)
El modelo usa tarifas planas reales por abordaje (no por km): Metro perfil Frecuente con Cívica **$3.820** y bus colectivo metropolitano **$3.800** (vigentes desde el 10-ene-2026, Área Metropolitana del Valle de Aburrá). La tarifa se cobra una sola vez por abordaje; ver `core/graph.py` (MODES) y `corpus/metro_medellin.md` para fuentes y perfiles (estudiantil, adulto mayor, eventual).

## Pendiente del equipo
1. Reemplazar coordenadas/parámetros por datos reales y citarlos (OSM/osmnx, AMVA, Metro, SIATA).
2. Completar el corpus. 3. Evaluar el agente. 4. Informe (4 págs.) y declaración de uso de IA generativa.
