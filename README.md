# Landscape Recovery Ontology (LRO)

A lightweight, standards-first ontology for **nature finance & landscape recovery** projects.

- **Prefix / Base IRI:** `lro:` → `https://greatyellow.earth/ontology#`
- **Primary serialization:** Turtle (`ontology/lro.ttl`)
- **Assumptions:** Currency **GBP**, CRS **EPSG:4326**, WKT and GeoJSON stored side-by-side.

> **Announcement / Press Release**  
> 🚀 **Introducing the Landscape Recovery Ontology (LRO)**  
> Today we’re open-sourcing a lightweight ontology for **nature finance & landscape recovery**. It reuses the good stuff—**SOSA/SSN** (observations), **GeoSPARQL 1.1** (geometries), **QUDT** (units), **OWL-Time**, **FOAF/ORG**, **DCTERMS/DCAT/SKOS**—and adds just enough glue for projects, parcels, habitats (UKHab), and measurements.  
>  
> Why care? LRO makes it easier to **find, join, and reason over** ecological & finance data:  
> • “All soil measurements in Project X, 2022–2025”  
> • “Total area under consideration”  
> • “Which landowners have the most chalk grassland?”  
> • “Area-weighted habitat condition by L1 UKHab”  
>  
> Get started in **Protégé** with the Turtle file, sample data, and ready-to-run SPARQL queries. Ontology & docs are **CC BY 4.0**; example code is **MIT**.  
>  
> 📣 We’d love feedback, issues, and contributions—especially mappings to UKHab codes and additional evaluation methods.  
>  
> #ontology #semanticweb #geospatial #biodiversity #naturefinance #geosparql #qudt #openscience

---

## Why another ontology?

This project glues together widely used vocabularies (SOSA/SSN, GeoSPARQL 1.1, OWL-Time, QUDT, FOAF/ORG, DCTERMS/DCAT/SKOS) with minimal new terms for project, parcel, habitat, observation, and finance concepts—aimed at semantic search and reporting for landscape recovery. See our original research spike for background and competency questions. :contentReference[oaicite:5]{index=5}

## Repo layout
```
landscape-recovery-ontology/
├─ README.md
├─ LICENSE # MIT (for code/examples)
├─ LICENSE-CC-BY-4.0 # CC BY 4.0 (for ontology/docs)
├─ CITATION.cff
├─ ontology/
│ └─ lro.ttl
├─ examples/
│ ├─ sample-data.ttl
│ └─ queries/
│ ├─ q1_soil_measurements.rq
│ ├─ q2_total_hectarage.rq
│ ├─ q3_chalk_grassland_owners.rq
│ └─ q4_area_weighted_condition.rq
└─ docs/
└─ ontology-diagram.png
```

## Install / Use

1. **Open in Protégé**  
   - File → Open… → `ontology/lro.ttl`
2. **Load the sample data**  
   - File → Import RDF… → `examples/sample-data.ttl`
3. **Run queries**  
   - Use SPARQL tab in Protégé to run examples from `examples/queries/`.

## Core dependencies (reused vocabularies)

- **SOSA/SSN** for observations/samples/procedures
- **GeoSPARQL 1.1** for features, geometries, and WKT/GeoJSON literals
- **QUDT** for units and quantities (including money)
- **OWL-Time** for project timelines
- **FOAF / ORG** for people & organizations
- **DCTERMS / DCAT / SKOS** for docs, datasets, and controlled code lists

References: SOSA/SSN, GeoSPARQL 1.1, QUDT, OWL-Time, DOAP (example of publishing), OBO/AgrO (licensing). :contentReference[oaicite:6]{index=6}

## Diagram

![Ontology diagram](docs/ontology-diagram.png)

## SPARQL examples (same as in `examples/queries/`)

- **Q1** – Soil measurements for *Project X* between 2022 and 2025  
- **Q2** – Aggregate hectarage for parcels under *Project X*  
- **Q3** – Landowners with the highest area of chalk grassland  
- **Q4** – Area-weighted average habitat condition by L1 UKHab in *Project X*

## Versioning & releases

- Version IRI: `https://greatyellow.earth/ontology/0.1.0`
- Changelog in GitHub Releases (tagged versions)
- Cite via `CITATION.cff`.

## License

- **Ontology & docs:** CC BY 4.0 (`LICENSE-CC-BY-4.0`)  
- **Code & examples:** MIT (`LICENSE`)

Rationale: Many ontologies (e.g., **AgrO**) use **CC BY 4.0** for the ontology; tooling frequently uses MIT/Apache-2.0 (e.g., **DOAP**, **OGC**). :contentReference[oaicite:7]{index=7}

## Contributing

Issues and pull requests are welcome—particularly:  
- SKOS expansion/mapping for **UKHab**  
- Additional **Evaluation Methods** and example datasets  
- Tests for query patterns and unit conversions via QUDT