# Landscape Recovery Ontology (LRO)

A lightweight, standards-first ontology for **nature finance & landscape recovery** projects.

- **Prefix / Base IRI:** `lro:` → `https://greatyellow.earth/ontology#`
- **Primary serialization:** Turtle (`ontology/lro.ttl`)
- **Assumptions:** Currency **GBP**, CRS **EPSG:4326**, WKT and GeoJSON stored side-by-side.

---

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
> Get started in **Protégé** (or **GraphDB**) with the Turtle file, sample data, and ready-to-run SPARQL queries. Ontology & docs are **CC BY 4.0**; example code is **MIT**.  
>  
> 📣 We’d love feedback, issues, and contributions—especially mappings to UKHab codes and additional evaluation methods.  
>  
> #ontology #semanticweb #geospatial #biodiversity #naturefinance #geosparql #qudt #openscience

---

## What’s new (since 0.1.x)

**Geospatial “Area” model & partonomy**
- New upper class `lro:Area ⊑ geo:Feature`.  
- `lro:Farm`, `lro:Site`, `lro:LandParcel`, `lro:Watershed` are all `lro:Area` (so they can all carry geometry).  
- Generic, transitive `lro:hasPart / lro:partOf` with domain-specific subproperties:  
  `lro:hasProject`, `lro:hasFarm`, `lro:hasSite`, `lro:hasLandParcel`.  
- **Property chain**: `lro:sampleOfParcel ∘ lro:partOf ⊑ lro:partOf` → a sample taken on a parcel is (by inference) part of the containing Site, Farm, Project, Programme.

**Farm is both place *and* business**
- `lro:Farm ⊑ lro:Area ∧ org:Organization ∧ schema:Place`.  
- People can be members of Farms/Companies/Projects via `lro:isMemberOf` / `lro:hasMember`.  
- For project-specific involvement you can also use `lro:participatesIn` / `lro:hasParticipant`.

**Sampling, observations, and parcel evaluations**
- A sample is tied to a parcel: `lro:sampleOfParcel(sosa:Sample → lro:LandParcel)`.  
- Observations point to the **sample**: `lro:observesSample ⊑ sosa:hasFeatureOfInterest`.  
- N-ary event pattern for **parcel evaluations**: `lro:ParcelEvaluation ⊑ sosa:Sampling` with  
  `lro:hasEvaluation( LandParcel→ParcelEvaluation )`,  
  `lro:usedEvaluationMethod ⊑ sosa:usedProcedure`,  
  `lro:hasEvaluationTime ⊑ time:hasTime`.  
  And `lro:EvaluationMethod ≡ sosa:Procedure`.

**Invoices without line-items**
- `lro:Invoice ⊑ schema:Invoice` with value via `lro:amount → qudt:QuantityValue`.  
- Generic `lro:invoiceFor` targets any billable thing (measurement, procedure, Site/Farm/Project, product/service)—no separate line-item class.

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
│ ├─ sample-data-extended.ttl
│ └─ queries/
│   ├─ q1_soil_measurements.rq
│   ├─ q2_total_hectarage.rq
│   ├─ q3_chalk_grassland_owners.rq
│   ├─ q4_area_weighted_condition.rq
│   ├─ q5_membership.rq # ← NEW
│   ├─ q6_samples_rollup.rq # ← NEW
│   ├─ q7_parcel_evaluations.rq # ← NEW
│   └─ q8_invoices.rq # ← NEW
├─ docs/
│ ├─ class-hierarchy-poc.svg
│ ├─ dependencies-poc.svg
│ └─ ontology-diagram.png
└─ shapes/
  └─ lro.shacl.ttl
```

## Install / Use

1. **Open in Protégé** (or **GraphDB**) 
   - File → Open… → `ontology/lro.ttl`
2. **Load the sample data**  
   - File → Import RDF… → `examples/sample-data.ttl`
3. **Run queries**  
   - Use SPARQL tab in Protégé to run examples from `examples/queries/`.
4) **Validate with SHACL (optional)**  
   Load `shapes/lro.shacl.ttl` in your triple store (e.g., GraphDB → Shapes). Validate dataset.

## Core dependencies (reused vocabularies)

- **SOSA/SSN** for observations/samples/procedures
- **GeoSPARQL 1.1** for features, geometries, and WKT/GeoJSON literals
- **QUDT** for units and quantities (including money)
- **OWL-Time** for project timelines
- **FOAF / ORG** for people & organizations
- **DCTERMS / DCAT / SKOS** for docs, datasets, and controlled code lists

References: SOSA/SSN, GeoSPARQL 1.1, QUDT, OWL-Time, DOAP (example of publishing), OBO/AgrO (licensing). :contentReference[oaicite:6]{index=6}

## Diagram

- See vizualisations under docs/

## Class Hierarchy (partial)
```
owl:Thing
├─ lro:Programme                (schema:Program)
├─ lro:Project                  (schema:Project)
├─ lro:Meeting                  (schema:Event)

├─ lro:Site                     (geo:Feature)
│  └─ (holds parcels via lro:hasParcel)

├─ lro:LandParcel               (geo:Feature)
│  ├─ (has geometry via geo:hasGeometry → geo:Geometry)
│  ├─ (classified via lro:hasUKHab → skos:Concept)
│  └─ (owner via lro:owner → foaf:Agent)

├─ lro:Watershed                (geo:Feature)
├─ lro:Location                 (dcterms:Location ∧ geo:Feature)

├─ lro:Visitor                  (foaf:Person)
├─ lro:Ecologist                (foaf:Person)
├─ lro:Investor                 (foaf:Person)
├─ lro:Farmer                   (foaf:Person)

├─ lro:Invoice                  (schema:Invoice)
│  └─ (uses lro:amount → qudt:QuantityValue)

├─ lro:MonetaryValue            (qudt:QuantityValue)

├─ lro:LegalDocument            (dcterms:Document)
├─ lro:FinancialModel           (dcterms:Document)
├─ lro:HydrologicalModel        (dcterms:Document)
├─ lro:TemplateDocument         (dcterms:Document)

├─ lro:HabitatType              (envo:ENVO_00002006)           ; ENVO habitat
├─ lro:EcosystemService         (owl:Class)
├─ lro:EcosystemFunction        (owl:Class)
├─ lro:EcosystemValue           (owl:Class)

├─ lro:SoilSample               (sosa:Sample)
├─ lro:WaterSample              (sosa:Sample)
├─ lro:AirSample                (sosa:Sample)

├─ lro:SpeciesObservation       (sosa:Observation)
│  └─ (links taxon via lro:observedSpecies → dwc:Taxon)

├─ lro:EvaluationMethod         (sosa:Procedure)
├─ lro:HabitatQualityAssessment (sosa:Observation)

# Reused “upper” classes (referenced, not expanded here):
#   geo:Feature, geo:Geometry, sosa:Observation, sosa:Sample, sosa:Procedure,
#   qudt:QuantityValue, time:Interval, dcterms:Document, dcat:Dataset,
#   foaf:Person, org:Organization, gn:Region, dwc:Taxon, envo:ENVO_00002006

```

## SPARQL examples / Competency Questions (same as in `examples/queries/`)

- **Q1**: Soil measurements for *Project X* between 2022 and 2025  
- **Q2**: Aggregate hectarage for parcels under *Project X*  
- **Q3**: Landowners with the highest area of chalk grassland  
- **Q4**: Area-weighted average habitat condition by L1 UKHab in *Project X*
- **Q5**: Which people are members of which Farms, Companies, or Projects?  
- **Q6**: Which Samples roll up (via the partonomy) to Sites, Farms, Projects, and Programmes?  
- **Q7**: For each LandParcel, which evaluation methods were used and when?  
- **Q8**: For each Invoice, what kind(s) of thing is it charging for (measurement, product, service, site, farm, project)?  

## Expected results (quick check)

> These results assume both datasets are loaded:
> - `examples/sample-data.ttl`
> - `examples/sample-data-extended.ttl`
>
> and a standard OWL 2 RL reasoner is enabled (needed for Q6 rollups).  
> Prefixes: `ex:` = `https://greatyellow.earth/example#`, `lro:` = `https://greatyellow.earth/ontology#`, `unit:` = `http://qudt.org/vocab/unit/`.

<details>
<summary><strong>Q1</strong> soil measurements in a project</summary>

| obs     | when (UTC)            | value | unit          |
|--------|------------------------|-------|---------------|
| ex:obs1 | 2023-05-10T09:00:00Z  | 2.8   | unit:PERCENT  |
</details>

<details>
<summary><strong>Q2</strong> total hectares under a project</summary>

| totalHectares |
|---------------|
| 21.2          |
</details>

<details>
<summary><strong>Q3</strong> chalk grassland owners</summary>

| owner          | hectares |
|----------------|----------|
| ex:AcmeFarms   | 12.5     |
</details>

<details>
<summary><strong>Q4</strong> area-weighted habitat condition (by UKHab L1)</summary>

| l1                          | areaWeightedAvgCondition                 |
|-----------------------------|------------------------------------------|
| lro:UKHab_L1_Grassland      | 0.691273584905660377358491               |
</details>

<details>
<summary><strong>Q5</strong> people & memberships / participation</summary>

| person  | personName   | affiliation             | type        |
|---------|--------------|-------------------------|-------------|
| ex:tim  | Tim Someone  | ex:GreatYellowCompany   | memberOf    |
| ex:tim  | Tim Someone  | ex:ELRProject           | memberOf    |
| ex:tim  | Tim Someone  | ex:ElmTreeFarm          | memberOf    |
</details>

<details>
<summary><strong>Q6</strong> samples that roll up to containers (via partonomy)</summary>

| sample      | container          | containerType   |
|-------------|--------------------|-----------------|
| ex:SampleA  | ex:SouthFieldSite  | lro:Site        |
| ex:SampleA  | ex:ElmTreeFarm     | lro:Farm        |
| ex:SampleA  | ex:ELRProject      | lro:Project     |
| ex:SampleA  | ex:ELRProgramme    | lro:Programme   |

<em>Note:</em> requires reasoning over <code>sampleOfParcel ∘ partOf ⊑ partOf</code>.
</details>

<details>
<summary><strong>Q7</strong> parcel evaluations (method & time)</summary>

| parcel      | eval      | method            | when (UTC)            |
|-------------|-----------|-------------------|-----------------------|
| ex:ParcelA  | ex:Eval1  | ex:VESS_Procedure | 2024-09-15T09:00:00Z  |

<em>Tip:</em> works either with the UNION pattern or with <code>lro:hasEvaluation owl:inverseOf lro:evaluatesFeature</code> + reasoning.
</details>

<details>
<summary><strong>Q8</strong> invoice kinds</summary>

| invoice       | kinds           |
|---------------|-----------------|
| ex:Invoice1   | measurement     |
| ex:Invoice2   | site, product   |

<em>Note:</em> Order in <code>GROUP_CONCAT</code> may vary by engine.
</details>

## Versioning & releases

- Version IRI: `https://greatyellow.earth/ontology/0.1.0`
- Changelog in GitHub Releases (tagged versions)
- Cite via `CITATION.cff`.

## License

This ontology and all original documentation are released under CC BY 4.0 (ontology & docs) and MIT (code/examples).
- **Ontology & docs:** CC BY 4.0 (`LICENSE-CC-BY-4.0`)  
- **Code & examples:** MIT (`LICENSE`)

Rationale: Many ontologies (e.g., **AgrO**) use **CC BY 4.0** for the ontology; tooling frequently uses MIT/Apache-2.0 (e.g., **DOAP**, **OGC**). :contentReference[oaicite:7]{index=7}

The LRO ontology reuses and references a number of existing vocabularies and code lists. Each retains its own licence, which applies when those terms are used:

SOSA/SSN, OWL-Time – W3C Recommendations (reuse encouraged under W3C terms).
GeoSPARQL 1.1 – OGC standard, openly reusable.
QUDT – © QUDT.org, licensed CC BY 4.0.
FOAF – Licensed under CC BY 1.0.
ENVO, EUNIS, SWEET – OBO Foundry ontologies, licensed CC0 unless otherwise noted.
FIBO – © EDM Council, licensed MIT.
UK Habitat Classification (UKHab) – The UKHab classification scheme is free-to-use under its own EULA. This ontology references UKHab codes as identifiers only and does not redistribute the full classification tables or definitions. Users wishing to apply UKHab in production must register with the UKHab partnership and agree to their terms of use: https://ukhab.org

**Disclaimer**
Great Yellow does not claim ownership of the external vocabularies or classifications listed above. Where UKHab or other third-party schemes are referenced, it is solely to support interoperability. Users are responsible for ensuring they comply with the relevant licence terms of those vocabularies when reusing or redistributing data.

## Contributing

Issues and pull requests are welcome—particularly:  
- SKOS expansion/mapping for **UKHab**  
- Additional **Evaluation Methods** and example datasets  
- Tests for query patterns and unit conversions via QUDT

## References

- Noy, N. F., & McGuinness, D. L. (2001). **Ontology Development 101: A Guide to Creating Your First Ontology.** Stanford KSL/Protégé. PDF. https://protege.stanford.edu/publications/ontology_development/ontology101.pdf  :contentReference[oaicite:0]{index=0}

- Ceccaroni, L., & Oliva, L. (2012). **Ontologies for the Design of Ecosystems.** In *Universal Ontology of Geographic Space: Semantic Enrichment for Spatial Data* (IGI Global). Chapter PDF/TOC: https://igiprodst.blob.core.windows.net/ancillary-files/9781466603271.pdf  (overview page: https://www.igi-global.com/viewtitle.aspx?TitleId=64001).  :contentReference[oaicite:1]{index=1}

- **BFO-2020** (Basic Formal Ontology) — GitHub repository for artifacts conformant with ISO/IEC 21838-2:2020: https://github.com/BFO-ontology/BFO-2020  :contentReference[oaicite:2]{index=2}

- Drakou, E. G., Lemmens, R. L. G., & Ayuninshih, F. (2019). **Designing an Ecosystem Services Ontology within GEOBON.** *Biodiversity Information Science and Standards* 3: e36338. DOI: 10.3897/biss.3.36338. (UTwente record: https://research.utwente.nl/en/publications/designing-an-ecosystem-services-ontology-within-geobon)  :contentReference[oaicite:3]{index=3}

- Bennett, B. (2010). **Foundations for an Ontology of Environment and Habitat.** In *FOIS 2010: Formal Ontology in Information Systems* (IOS Press), pp. 31–44. (ACM/IOS references and metadata)  https://dl.acm.org/doi/proceedings/10.5555/1804715  :contentReference[oaicite:4]{index=4}

- Buttigieg, P. L., et al. (2016). **The environment ontology in 2016: bridging domains with increased scope, semantic density, and interoperation.** *Journal of Biomedical Semantics*. https://jbiomedsem.biomedcentral.com/articles/10.1186/s13326-016-0097-6  :contentReference[oaicite:5]{index=5}

- Affinito, F., Holzer, J. M., Fortin, M.-J., & Gonzalez, A. (2025). **Towards a unified ontology for monitoring ecosystem services.** *Ecosystem Services*. ScienceDirect article page: https://www.sciencedirect.com/science/article/pii/S2212041625000300  :contentReference[oaicite:6]{index=6}

- Lepczyk, C. A., Lortie, C. J., & Anderson, L. J. (2008). **An ontology for landscapes.** *Ecological Complexity*, 5(3), 272–279. https://doi.org/10.1016/j.ecocom.2008.04.001  :contentReference[oaicite:7]{index=7}
