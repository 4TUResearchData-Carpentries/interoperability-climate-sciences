---
title: "Semantic interoperability"
teaching: 30
exercises: 15
---

:::::::::::::::::::::::::::::::::::::: questions

* What is semantic interoperability?
* What types of meaning must be made explicit to enable semantic interoperability?
* Why can two structurally similar datasets still be scientifically incompatible?
* What is the difference between a label, a controlled vocabulary, a code list, and an ontology?
* Where can researchers discover, evaluate, and share semantic artefacts for the Earth sciences?
* How do the CF Conventions encode the meaning and context of climate and atmospheric data?
* Is using the same CF `standard_name` sufficient to make two variables directly comparable?
* What does it mean for a NetCDF file to conform to a particular version of the CF Conventions?
* What can a CF compliance checker determine, and what are its limitations?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

By the end of this episode, learners will be able to:

* distinguish structural interoperability from semantic interoperability;
* identify the semantic information needed to interpret and compare scientific variables;
* distinguish free-text labels from controlled vocabulary terms and formally defined relationships;
* use an Earth-science semantic-artefact catalogue to discover and critically assess relevant ontologies and vocabularies;
* explain how CF standard names, units, coordinates, bounds, grid mappings, and cell methods work together;
* evaluate whether two variables with similar names are semantically and scientifically comparable;
* interpret CF compliance-checker findings critically, including version mismatches and tool limitations; and
* identify semantic gaps in the IDRA radar datasets that may need harmonisation before comparison.

::::::::::::::::::::::::::::::::::::::::::::::::

## What is semantic interoperability?

Semantic interoperability concerns **shared and explicit meaning**.

A dataset is semantically interoperable when different people and software systems can interpret its variables, categories, relationships, and measurement context consistently because those meanings are expressed using documented, community-agreed terms and rules.

A useful guiding question is:

> **Can another researcher or software tool determine what the values represent, under which conditions they were produced, and how they relate to other data without relying mainly on tacit knowledge?** 

Or in other words : Can others understand and reuse the data correctly without making unnecessary assumptions?

This definition does not require every explanation to be written directly inside one file. Meaning may also be expressed through persistent identifiers that resolve to external vocabularies, code lists, specifications, instruments, methods, or provenance records. What matters is that the references and relationships are explicit and machine-actionable rather than hidden in personal knowledge, filenames, or informal documentation.

Semantic interoperability supports questions such as:

* What physical quantity is represented?
* What entity, medium, or phenomenon does the quantity concern?
* What is the direction or reference frame?
* At which height, depth, pressure level, or location was it observed?
* Does each value represent a point measurement, mean, sum, minimum, maximum, or another statistic?
* Over which spatial or temporal interval was it calculated?
* Which calendar, coordinate reference system, or vertical datum is used?
* Which quality flag, uncertainty estimate, instrument, or processing step applies?
* Are two variables equivalent, convertible, related, or fundamentally different?

## Structure and meaning are different—but connected

Structural interoperability and semantic interoperability address different questions.

| Question | Primarily structural | Primarily semantic |
|---|---:|---:|
| Does a variable exist, and what is its data type? | ✓ | |
| Which dimensions does the variable use? | ✓ | |
| Where is the `units` attribute stored? | ✓ | |
| What physical quantity does the variable represent? | | ✓ |
| Is `K` dimensionally compatible with `degC`? | | ✓ |
| Does `time: mean` describe an average rather than an instantaneous value? | | ✓ |
| Which coordinate variable applies to a data variable? | ✓ | ✓ |
| Does a height coordinate refer to metres above ground, sea level, or another reference surface? | | ✓ |
| Is a quality-control variable explicitly related to the measurement variable? | ✓ | ✓ |

The boundary is not always absolute. A metadata convention often defines both:

* **structural rules**, such as where an attribute must occur or how a related variable is referenced; and
* **semantic rules**, such as what the permitted term means.

For example, the presence and location of a `cell_methods` attribute are structural. The distinction between `time: point`, `time: mean`, and `time: sum` is semantic.

## Semantic resources for expressing shared meaning

Scientific meaning can be expressed using different types of **semantic resources**. These resources vary in how formally they define terms and relationships.

Common examples include:

* **Free-text labels** — human-readable descriptions, such as `long_name = "surface temperature"`. They provide useful context but are not necessarily standardised.

* **Controlled vocabularies** — agreed sets of terms with documented definitions, such as the **CF Standard Name Table**.

* **Code lists** — predefined values or codes used for a particular property or category.

* **Taxonomies and thesauri** — collections of concepts organised through relationships such as broader, narrower, or related terms.

* **Ontologies** — formal representations of concepts and the relationships between them, enabling richer machine-readable descriptions.

* **Persistent identifiers and qualified references** — links that connect a dataset or metadata element to an authoritative definition, instrument, method, vocabulary term, or other related resource.

These resources are not interchangeable. The appropriate choice depends on **what meaning needs to be expressed and how precisely software needs to interpret it**.


The [CF Standard Name Table](https://cfconventions.org/Data/cf-standard-names/current/build/cf-standard-name-table.html) is a controlled vocabulary. It defines standard names, descriptions, and canonical units. It is not, by itself, a full ontology of climate science.

A semantic resource is more useful when:

* its terms have stable identifiers;
* definitions are publicly accessible;
* versions and changes are documented;
* synonyms and deprecated terms are managed;
* relationships between concepts are explicit where needed; and
* the resource is maintained through an open community process.

This aligns with the FAIR Interoperability principles: data and metadata should use formal, shared knowledge-representation languages, FAIR vocabularies, and qualified references to related data and metadata.


::::::::::::::::::::::::::::::::::::: callout

### Relevant resource: EarthPortal

[EarthPortal](https://earthportal.eu/) is a catalogue and repository for ontologies and other semantic artefacts in Earth-system, environmental, and related domains. It can help researchers, data stewards, and infrastructure developers move beyond locally invented labels by discovering semantic resources that may already exist in their community.

EarthPortal provides several ways to explore and evaluate semantic artefacts:

* [Browse the ontology catalogue](https://earthportal.eu/ontologies) and filter resources by Earth-science category, group, language, representation format, or semantic-resource type.
* [Search ontology content](https://earthportal.eu/search) to find concepts across multiple ontologies rather than searching only by ontology title.
* Use the [Recommender](https://earthportal.eu/recommender) to identify potentially relevant ontologies from a sample of terms or text.
* Use the [Annotator](https://earthportal.eu/annotator) to identify ontology concepts that may describe terms occurring in documentation or metadata.
* Inspect mappings, identifiers, classes, properties, provenance, submissions, and available machine-readable representations.
* After creating an account and signing in, use **Submit ontology** to share an ontology or another semantic artefact with the wider Earth-science community.

A useful search exercise is to look for terms such as `air_temperature`, `precipitation_flux`, or radar-related quantities and compare what EarthPortal exposes with the current CF Standard Name Table.

:::::::::::::::::::::::::::::::::::::

## Semantic interoperability is not provided by a file format

NetCDF, Zarr, CSV, TSV, Parquet, GeoTIFF, and other formats can all carry data that are semantically clear - or semantically ambiguous.

For example, a CSV table may contain:

```text
station,time,RR
Cabauw,2026-07-14T08:00:00Z,0.3
```

The table is structurally simple, but `RR` remains ambiguous unless a schema or metadata record states:

* the controlled concept represented by `RR`;
* whether the value is precipitation amount, rainfall depth, or precipitation rate;
* the unit;
* the time interval;
* whether the value is instantaneous, accumulated, averaged, or derived;
* the station identifier and coordinate reference; and
* how missing and quality-controlled values are represented.

A Parquet schema can define `RR` as a floating-point column, but a data type does not define the scientific quantity. A NetCDF variable can carry attributes, but the presence of attributes does not ensure that they use shared terms correctly.

The file format provides a place to encode meaning. A community convention supplies the semantic contract.

## Semantic interoperability requires context, not only names and units

Consider two variables:

```text
float temperature(time, latitude, longitude);
    temperature:standard_name = "air_temperature";
    temperature:units = "K";
```

and:

```text
float tas(time, latitude, longitude);
    tas:standard_name = "air_temperature";
    tas:units = "degC";
```

The variable names differ, but the shared `standard_name` indicates the same physical quantity. Their units are convertible, so software can harmonise them.

However, this does **not** yet prove that the variables can be directly combined. Ash must still inspect:

* the height or pressure level of the air temperature;
* coordinate systems and locations;
* time coordinates and calendars;
* spatial and temporal resolution;
* whether values are instantaneous or averaged;
* cell bounds and aggregation intervals;
* observation versus model context;
* quality flags and uncertainty;
* calibration and processing level; and
* missing-data and validity rules.

Semantic interoperability establishes interpretable meaning and relationships. It does not automatically establish that two datasets are scientifically interchangeable or suitable for a particular analysis.


::::::::::::::::::::::::::: challenge

## Can these variables be compared? (Think–Pair–Discuss)

Consider the following variables.

### Dataset A

```text
standard_name = "air_temperature"
units = "K"
height = 2 m
cell_methods = "time: point"
```

### Dataset B

```text
standard_name = "air_temperature"
units = "degC"
height = 2 m
cell_methods = "time: mean"
time_bounds = one-hour intervals
```

### Dataset C

```text
standard_name = "surface_temperature"
units = "K"
cell_methods = "time: point"
```

Discuss:

1. Which pairs describe the same physical quantity?
2. Which units are convertible?
3. Which variables can be directly compared without further processing?
4. What harmonisation would be required?
5. Which information is semantic, and which is structural?

::::::::: solution

## Solution

**Datasets A and B**

They use the same standard name and refer to air temperature at the same height. Their units are convertible. However, Dataset A represents point values while Dataset B represents one-hour means. They should not be treated as directly equivalent until their temporal representation is harmonised—for example, by calculating comparable hourly means from Dataset A, if its sampling supports that operation.

**Datasets A and C**

Their units are compatible, but the standard names identify different quantities. `air_temperature` and `surface_temperature` must not be treated as synonyms.

**Datasets B and C**

The variables differ in physical quantity and temporal treatment, so unit conversion alone is insufficient.

**Structural information**

The existence and location of attributes, the link to `time_bounds`, and the shapes and dimensions of variables are structural.

**Semantic information**

The definitions of `air_temperature`, `surface_temperature`, `time: point`, `time: mean`, the height reference, and the interpretation of units are semantic.

:::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::

## Semantic interoperability through the CF Conventions

The **Climate and Forecast (CF) Metadata Conventions** provide standard ways to describe the meaning and context of variables in NetCDF datasets. This helps researchers and software determine what the data represents and where and when it applies.

Some of the main CF elements are:

* **`standard_name` — What is being measured?**
  Gives a variable a standardised name with a defined scientific meaning, such as `air_temperature` or `precipitation_flux`.

* **`long_name` — How can we describe it for people?**
  Provides a human-readable description of the variable.

* **`units` — How are the values expressed?**
  Specifies the physical units of the variable. When a `standard_name` is used, the units must be physically compatible with its canonical units.

* **Coordinates — Where and when?**
  Describe the spatial and temporal location of the data, including latitude, longitude, vertical position, and time.

* **`bounds` and `cell_methods` — What does each value represent?**
  `bounds` can describe the extent of a coordinate interval, while `cell_methods` describes operations such as a mean, sum, or maximum over space or time.

* **`grid_mapping` — How is the data located on Earth?**
  Describes the coordinate reference system or map projection used by the dataset.

* **Ancillary variables and flags — What additional information is associated with the values?**
  Can describe information such as measurement uncertainty, instrument status, or quality-control flags.

* **`featureType` — What kind of observations are these?**
  Describes discrete sampling geometries such as time series, profiles, or trajectories.

Together, these elements make the scientific meaning and context of the data more explicit and machine-readable.


:::: callout

#### Explore further: the Climate and Forecast ontology representation

EarthPortal includes the [Climate and Forecast (CF) features ontology](https://earthportal.eu/ontologies/CFF), an OWL representation of generic features derived from the CF Standard Names vocabulary. It exposes CF-related concepts through ontology classes, properties, individuals, identifiers, and relationships that can be explored programmatically or through the portal interface.

This resource illustrates the difference between:

1. the **authoritative CF Standard Name Table**, which governs the standard names used in CF-compliant datasets; and
2. an **ontology representation**, which expresses selected CF concepts and relationships in a formal knowledge-representation language.

The ontology representation may support linked-data exploration, mappings, semantic annotation, and integration with other ontologies. However, it should not automatically be treated as a replacement for the current [CF Standard Name Table](https://cfconventions.org/Data/cf-standard-names/current/build/cf-standard-name-table.html) or the [CF Conventions](https://cfconventions.org/).


#### CF is a convention, not a complete description of every research context

CF is powerful, but CF compliance does not guarantee:

* that the scientific values are correct;
* that calibration or processing was appropriate;
* that uncertainty is adequately described;
* that all provenance is available;
* that discovery metadata are complete;
* that two datasets use the same spatial or temporal resolution;
* that two variables are suitable for the same research question;
* that missing values are acceptably limited; or
* that a dataset is free from software or production errors.

::::::::::::::

Other standards and metadata profiles may complement CF by providing richer dataset level discovery and citation metadata. For example:

* dataset-discovery metadata may be expressed through Attribute Convention for Data Discovery (ACDD, see [Glossary](../learners/reference.md)), ISO 19115 (Geographic information metadata , see [Glossary](../learners/reference.md)), DataCite (see [Glossary](../learners/reference.md)), or repository metadata;
* instruments and observation procedures may require domain-specific vocabularies or provenance models;
* persistent identifiers can connect datasets to instruments, software, methods, publications, and derived products.

Semantic interoperability is therefore layered. CF provides an important domain convention, not the totality of scientific meaning.

::::::::::::::::::::::::::::::::::::: challenge

## Semantic interoperability: True or False?

Indicate whether each statement is True or False, and justify your answer.

1. A NetCDF file with dimensions, variables, and units is semantically interoperable by default.
2. CF standard names allow software to distinguish different kinds of temperature.
3. Semantic interoperability mainly benefits human readers, not automated workflows.
4. Two datasets using the same CF standard name can always be compared directly without further interpretation.
5. Semantic interoperability can be achieved reliably through locally invented variable names, without community-agreed definitions.
6. A descriptive `long_name` has the same machine-actionable status as a valid CF `standard_name`.
7. Passing a compliance checker proves that a dataset is scientifically correct and fully interoperable.
8. Variables expressed in `K` and `degC` may be convertible while still requiring additional contextual harmonisation.

:::::::::::::::::::::::::::::::::::::::::::::: solution

1. **False.** NetCDF provides a self-describing structure, but dimensions and units do not fully define scientific meaning, context, statistical treatment, or relationships.

2. **True.** CF standard names distinguish concepts such as `air_temperature`, `sea_surface_temperature`, and `surface_temperature` through controlled terms and definitions.

3. **False.** Shared semantics enable automated discovery, validation, unit conversion, subsetting, comparison, and integration.

4. **False.** A shared standard name is strong evidence that variables represent the same physical quantity, but direct comparison still depends on coordinates, units, heights or depths, cell methods, bounds, resolution, quality, provenance, and processing context.

5. **False.** Local names may be understandable within one project, but reliable interoperability requires mappings to shared definitions, vocabularies, or identifiers.

6. **False.** `long_name` is normally free text. A CF `standard_name` must be selected from the governed Standard Name Table and has a defined meaning and canonical unit.

7. **False.** A checker evaluates implemented conformance rules. It does not verify scientific correctness, data quality, completeness of all relevant metadata, or suitability for a specific analysis.

8. **True.** Unit conversion may be possible, but the variables may still differ in height, aggregation, calendar, coordinate system, processing level, or measurement method.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::

## What does CF-compliant mean?

A NetCDF file is CF-compliant **relative to a declared CF version** when it satisfies the requirements of that version and uses CF terms according to their defined meanings.

For example:

```text
:Conventions = "CF-1.13";
```

The global `Conventions` attribute is a declaration by the data producer. It is not proof by itself. A file may declare a convention while still containing invalid standard names, incompatible units, missing coordinate metadata, or incorrectly expressed relationships.

CF documents distinguish between:

* **requirements**, which must be satisfied for conformance; and
* **recommendations**, which improve interoperability but are not always mandatory.

A compliance assessment should therefore report:

1. the CF version claimed by the file;
2. the CF version against which it was evaluated;
3. requirements that fail;
4. recommendations that are not followed;
5. warnings or implementation limitations; and
6. which checker and checker version produced the report.

::::::::::::::::::::::::::::::::::::: callout

### Compliance is version-specific

A file written for an older CF version should ideally be evaluated against that version. Running a newer checker may still reveal useful interoperability problems, but it does not retroactively determine whether the file conformed to every rule of its originally declared version.

Similarly, a checker may implement only part of a convention. The CF specification remains the authoritative source.

:::::::::::::::::::::::::::::::::::::

## Inspecting semantic metadata in the IDRA radar files

The two IDRA files used in this lesson declare:

```text
Conventions = "CF-1.4"
```

They contain useful structural and descriptive metadata, including time coordinates, units, `long_name` values, comments, and fill values. However, several radar data variables—such as:

```text
equivalent_reflectivity_factor
differential_reflectivity
radial_velocity
spectrum_width
differential_phase
```

are primarily described through local variable names, free-text `long_name` values, units, and comments.

This creates useful questions for semantic assessment:

* Does each local variable name correspond to a valid CF standard name?
* If a matching standard name exists, is it recorded in the `standard_name` attribute?
* Are units written in a valid and unambiguous UDUNITS form?
* Does `ms-1` express the intended velocity unit, or should it be written as `m s-1` or `m/s`?
* Are azimuth, elevation, range, and radar position represented through sufficient coordinate semantics?
* Is the positive direction of radial velocity explicit?
* Are measurement uncertainties or quality flags linked to the data variables?
* Do the files provide enough metadata to distinguish point measurements, averages, and derived products?
* Does declaring `CF-1.4` accurately describe how all variables use the convention?

The purpose is not to conclude that the datasets are unusable. They are structurally similar and richly documented for human readers. The purpose is to identify which meanings are machine-actionable and which still depend on domain knowledge or free-text interpretation.

::::::::::::::::::::::::::::::::::::: challenge

## Ash's semantic-comparison checklist

Ash wants to compare `equivalent_reflectivity_factor` and `radial_velocity` between the 2009 and 2019 IDRA files. One from [27 April 2009](https://opendap.4tu.nl/thredds/dodsC/IDRA/2009/04/27/IDRA_2009-04-27_06-08_raw_data.nc.html) and one from [2 January 2019](https://opendap.4tu.nl/thredds/dodsC/IDRA/2019/01/02/IDRA_2019-01-02_12-00_raw_data.nc.html).

Before combining the values, decide whether she has enough information to answer the following questions.

| Question | Metadata element to inspect | Why it matters |
|---|---|---|
| Do both variables represent the same physical quantity? | `standard_name`, `long_name`, definition or vocabulary mapping | Similar local names are not proof of semantic equivalence |
| Are units valid and compatible? | `units`, canonical units, UDUNITS parsing | Strings that look similar may be invalid or ambiguous |
| Is the sign convention the same? | standard-name definition, comments, reference direction | Opposite sign conventions can reverse interpretation |
| Do values represent the same temporal treatment? | `cell_methods`, time bounds, sampling information | Point values and means are not equivalent |
| Are measurements located in the same coordinate frame? | range, azimuth, elevation, station position, CRS or grid mapping | Radar bins need spatial interpretation |
| Are missing and invalid values handled consistently? | `_FillValue`, `missing_value`, `valid_range`, quality flags | Invalid values must not enter comparisons |
| Are processing and calibration comparable? | `history`, provenance, processing-level metadata, instrument information | Identical names do not guarantee identical processing |
| Is uncertainty represented? | `ancillary_variables`, uncertainty variables, quality flags | Differences may be smaller than measurement uncertainty |

::::::::: solution

The two files use nearly identical variable names, dimensions, units, and comments, which strongly supports comparison using a shared workflow. However, semantic comparability should not be inferred from names alone.

Ash can establish more reliable comparability by:

* validating or mapping the local radar variable names to controlled terms;
* confirming unit syntax and compatibility;
* documenting sign conventions and measurement geometry;
* checking whether temporal and spatial sampling are equivalent;
* comparing calibration, processing, and noise information;
* applying missing-value and quality-control rules consistently; and
* recording any assumptions made during harmonisation.

:::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::::


::::: callout

## The IOOS Compliance Checker

The [IOOS Compliance Checker](https://compliance.ioos.us/index.html) is a Python-based tool that evaluates local or remote NetCDF datasets against implemented metadata standards, including selected versions of CF. Its source code and documentation are available through the [IOOS Compliance Checker project](https://ioos.github.io/compliance-checker/).

The checker is useful for identifying potential conformance problems, but its own documentation states that it should be used as guidance rather than treated as the authoritative determination of complete compliance.

### Run the assessment

1. Open the [IOOS Compliance Checker](https://compliance.ioos.us/index.html).

2. Inspect the file before running the checker, to see which version of CF is using. 

3. Select an available CF test version. Prefer a relatively close compatibility assessment. 

4. Upload the .nc dataset. You can also provide a direct remote OPeNDAP data URL, if using the [CLI](https://ioos.github.io/compliance-checker/readme_link.html#the-compliance-checker-command-line-tool).

5. Submit the dataset and download or save the report.

6. Classify each finding into one of the following categories:

   * invalid or missing controlled-vocabulary term;
   * invalid or incompatible unit;
   * coordinate or reference-system problem;
   * missing relationship between variables;
   * missing statistical or interval context;
   * recommendation for human-readable or discovery metadata;
   * checker limitation or version mismatch.


### Interpretation questions

* Which CF version does the dataset claim?
* Which CF version did the checker actually evaluate?
* Which findings are errors, and which are recommendations or warnings?


::::::::::::::::::::::



:::::::::: keypoints

* Semantic interoperability concerns shared, explicit, and machine-actionable meaning.
* File formats and readable labels provide containers for meaning but do not guarantee semantic agreement.
* Catalogues such as EarthPortal support the discovery, assessment, mapping, and sharing of Earth-science semantic artefacts, but users must still evaluate authority, versioning, provenance, licensing, and community adoption.
* Controlled vocabulary terms, units, coordinates, bounds, cell methods, grid mappings, flags, and qualified relationships work together to express scientific meaning.
* A CF `standard_name` identifies a physical quantity; `long_name` remains free text for human readability.
* The same standard name and convertible units are not sufficient to guarantee direct scientific comparability.
* CF compliance is relative to a specific version and does not prove scientific correctness, data quality, or suitability for a particular analysis.
* The global `Conventions` attribute is a conformance claim, not evidence that every requirement is satisfied.
* Compliance checkers evaluate implemented rules and must be interpreted alongside the authoritative specification and domain knowledge.
* The IDRA files are structurally similar and well documented for human readers, but some radar semantics may still require controlled mappings, clearer unit syntax, measurement-context metadata, and provenance.
* Semantic interoperability is achieved through community-agreed definitions, stable identifiers, explicit relationships, and transparent harmonisation—not through variable names alone.

::::::::::