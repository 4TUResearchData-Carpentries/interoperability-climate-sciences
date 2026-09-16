---
title: "Interoperable Infrastructure in the AI Era"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

What does “AI-ready” mean in the context of climate and atmospheric data?

How do structural, semantic, and technical interoperability support AI workflows?

Why do large-scale AI workflows place additional demands on data infrastructure?

What can interoperability enable for AI, and what does it not guarantee?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

By the end of this episode, learners will be able to explain AI readiness as a property of a particular data workflow rather than of a file format alone.

Learners will be able to connect the structural, semantic, and technical interoperability concepts introduced throughout this lesson to the requirements of automated AI workflows.

Learners will also be able to distinguish problems that interoperability can address from broader questions of data quality, suitability, provenance, and scientific validity.

::::::::::::::::::::::::::::::::::::::::::::::::


## Why end an interoperability course with AI?

Throughout this lesson, we have looked at interoperability from three complementary perspectives. Structural interoperability concerns how data are organised and represented. Semantic interoperability concerns whether their scientific meaning is expressed consistently and explicitly. Technical interoperability concerns whether systems can access and exchange those data programmatically. The previous episode added another important consideration: **scale**. Cloud-native layouts such as Zarr change how large multidimensional datasets can be organised and retrieved, making selective and parallel access easier in suitable infrastructures.

Artificial intelligence does not introduce a fourth interoperability layer. Instead, AI makes the consequences of the existing interoperability layers particularly visible. A researcher working interactively with a small dataset may be able to rename a variable manually, read a README to discover its units, download several files through a browser, or correct an inconsistent coordinate before continuing the analysis. An automated training pipeline processing thousands or millions of samples cannot rely easily on those kinds of manual interventions. The more automated and data-intensive the workflow becomes, the more important it is that structure, meaning, and access are explicit and machine-actionable. In this sense, AI provides a useful **stress test for interoperability**.


## What does “AI-ready” mean?

There is no single file format, metadata convention, or storage technology that automatically makes scientific data AI-ready.

For this lesson, we will use the following working definition: **AI-ready data infrastructure enables a defined machine-learning workflow to discover, access, interpret, retrieve, transform, and trace the required data with minimal dataset-specific manual intervention.** The word **defined** is important. AI readiness is task-dependent.

A global temperature dataset may be perfectly suitable for training one forecasting model but unsuitable for another model that requires hourly precipitation, higher spatial resolution, labelled extreme events, or additional atmospheric variables. Similarly, converting a NetCDF dataset to Zarr may make repeated remote access more efficient, but it does not tell a model what the variables mean. Adding CF metadata may make those variables easier to interpret, but it does not guarantee that the observations provide suitable training examples. AI readiness therefore builds on interoperability, but extends beyond it.


## Following Ash's data into an AI workflow

Earlier in the lesson, Ash wanted to compare radar observations from two IDRA datasets. She had to determine whether the datasets could be accessed, whether they had compatible structures, and whether their variables could be interpreted consistently. Imagine that Ash now wants to go further.

Instead of comparing two files, she wants to train a model that predicts extreme rainfall events using several years of radar observations together with atmospheric and reanalysis data. Her workflow may need to retrieve many thousands of small spatial and temporal subsets repeatedly during data preparation and model training. The questions Ash encountered earlier have not disappeared. They have become part of an automated pipeline.

| Question the workflow must answer | Interoperability concept |
| --- | --- |
| Can software locate the arrays, dimensions and coordinates consistently? | Structural interoperability |
| Does `precipitation`, `reflectivity`, or `temperature` represent the same scientific quantity across datasets? | Semantic interoperability |
| Can the required data be retrieved programmatically without manual downloads? | Technical interoperability |
| Can only the required parts of very large datasets be retrieved efficiently? | Structural and technical design, including cloud-native access patterns |
| Can Ash determine exactly which data and processing steps produced the training dataset? | Provenance, identification and versioning supporting reproducibility |

The first three questions should now be familiar. They correspond directly to the interoperability layers developed throughout this lesson. The last two show why AI also places pressure on the wider infrastructure around the data.


## AI changes the scale of the interoperability problem

Machine-learning workflows often access data differently from traditional file-based analysis. A researcher may download a NetCDF file once and analyse most of its contents. An AI pipeline may instead request many small slices from a large collection repeatedly during preprocessing, training, validation, and evaluation. Interoperability supports this entire flow.

A predictable data model allows software to locate the required arrays. Shared semantic conventions allow software and researchers to interpret those arrays consistently. Standard access mechanisms allow the pipeline to retrieve them automatically. For very large collections, the physical organisation of the data also affects performance. Chunk-oriented layouts such as Zarr can allow different portions of multidimensional arrays to be retrieved independently and in parallel from object storage.

But there is no requirement that every AI workflow use Zarr. A NetCDF archive exposed through OPeNDAP may be entirely appropriate for some workflows. Existing NetCDF files may also be exposed through a Zarr-compatible access model using approaches such as Kerchunk.

As we saw in the previous episode, the appropriate infrastructure depends on the storage environment, access pattern, dataset size, and computational workflow.


### From interoperable source data to AI-ready training data

An important distinction is the difference between an **interoperable scientific dataset** and a **task-specific AI training dataset**.

Suppose Ash finds a collection of well-structured NetCDF datasets. The variables use CF metadata and can be accessed programmatically through OPeNDAP. Those datasets already provide strong structural, semantic, and technical interoperability.

Ash may nevertheless need to resample observations onto a common temporal grid, align spatial coordinates, convert units, handle missing measurements, derive rainfall labels, or select particular variables before the data can be used to train her model. This distinction is important because **AI readiness should not require repositories to anticipate every future machine-learning task**. A research infrastructure can instead provide well-described, accessible, interoperable source data from which researchers can construct specialised training datasets reproducibly.


### Interoperability reduces hidden assumptions

Consider what happens when interoperability is weak. A training script may assume that every variable called `precipitation` has the same definition. A preprocessing step may assume that time coordinates use the same calendar. A model may combine values expressed using different units. A workflow may silently use a newer version of a dataset when an experiment is repeated.

These problems are particularly difficult in AI workflows because the transformation from source observations to model inputs may involve very large numbers of records. An inconsistency that would be obvious when inspecting ten observations manually may become a systematic error when propagated through millions of training samples. Interoperability reduces the number of assumptions that need to remain hidden in code or in the researcher's knowledge.


### Interoperability does not guarantee trustworthy AI

Interoperability is an important foundation for automated and reproducible AI workflows, but it should not be confused with scientific validity or model quality. A dataset can be structurally, semantically, and technically interoperable while still being unsuitable for a particular AI application. For example, observations may contain measurement errors. Important geographic regions or rare events may be poorly represented. Training and evaluation datasets may not represent the same population. Labels may be uncertain. Missing observations may introduce systematic patterns. A model may learn relationships that do not generalise beyond the training period.

These are questions of **data quality, representativeness, modelling methodology, validation, uncertainty, and scientific interpretation**. Interoperability does not solve them.

What interoperability does is make the data and their context easier to access, combine, inspect, process, and trace. That creates better conditions for identifying and addressing such problems.



### Reproducibility across the infrastructure

Large automated workflows introduce another requirement: Ash must be able to determine **which data produced a particular model**.

Imagine that a dataset is corrected after Ash trains her model. If she returns to the same repository six months later, can she identify the version she originally used? If her preprocessing converted units, removed observations, resampled time coordinates, or generated derived variables, can those decisions be reconstructed? Persistent identifiers, dataset versions, provenance records, processing parameters, software environments, and workflow descriptions help connect an AI model back to the data and transformations from which it was produced.

These elements should be understood as **cross-cutting reproducibility infrastructure** rather than simply another form of technical interoperability.The model is therefore not an isolated research object. It sits at the end of a chain of data, metadata, software, and transformations.


## Real-world movement towards AI-ready Earth-system data

This shift is already visible in climate and Earth observation infrastructures. Initiatives such as [**FAIR-EO**](https://eodata.bvlabs.ai/ai4eo/) are developing environments in which standardised and semantically annotated Earth observation datasets are connected with AI models, analysis pipelines, and experimental results. In weather and climate prediction, [**ECMWF's Anemoi framework**](https://www.ecmwf.int/en/about/media-centre/aifs-blog/2026/anemoi-european-framework-ai) similarly treats the creation and cataloguing of curated training datasets as part of a larger reproducible workflow connecting Earth-system data, data preparation, distributed training, models, and inference. These initiatives illustrate an important point: AI-ready infrastructure is not simply about placing large datasets close to GPUs. It is about connecting **data, metadata, access mechanisms, processing workflows, provenance, and computing infrastructure** so that scientific data can move through increasingly automated workflows without losing their structure, meaning, or history.


## Exercise — Can Ash's workflow become AI-ready? (10 min)

:::::::::::::::::::::::::::::::::::::: challenge

Ash now wants to use several years of radar data together with reanalysis data to train a model for extreme rainfall prediction.

The radar archive contains NetCDF files from different years. Some older files use `time` while newer files use `time_processed_data` for a comparable dimension. A rainfall-related variable has a descriptive `long_name`, but no community-defined standard name, and part of its measurement context is explained only in documentation.

Recent radar files can be accessed through OPeNDAP, while some older observations must still be downloaded manually from another archive. The reanalysis data are available as CF-described Zarr datasets through object storage.

Ash writes a preprocessing notebook that aligns the datasets and creates the arrays required for model training. However, the notebook does not record the exact versions of the source datasets or all parameters used during preprocessing. Finally, the radar observations contain substantial gaps during some extreme rainfall events.

Think individually about this scenario for approximately two minutes. Then discuss it with a partner.

Identify where you see a **structural interoperability problem**, a **semantic interoperability problem**, and a **technical interoperability problem**.

Then identify problems that affect **AI readiness or reproducibility but are not solved simply by improving interoperability**.

Finally, decide what Ash should address before treating the resulting training dataset as ready for her experiment.

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::: solution

The inconsistent time dimension is a structural interoperability problem. Software cannot assume that the same logical dimension will always appear under the same structure or identifier. Ash may need a documented harmonisation step or a common schema before the files can participate reliably in one automated workflow.

The rainfall variable presents a semantic interoperability problem. A descriptive name may help Ash understand the variable, but the pipeline still needs sufficient machine-actionable information about what the quantity represents, its units, coordinate context, measurement method, and any processing that affects its interpretation. A community convention such as CF can reduce this ambiguity where appropriate.

The older archive presents a technical interoperability problem. Manual downloading creates a discontinuity in an otherwise automated workflow. Providing the archive through a standard remote-access mechanism would make it easier for the same workflow to operate across the complete collection.

The missing dataset versions and preprocessing parameters are primarily reproducibility and provenance problems. Even if every source dataset were interoperable, Ash could have difficulty reconstructing the exact training data used for a particular experiment.

The gaps during extreme rainfall events raise a different issue again. They concern the suitability and representativeness of the training data. Structural, semantic, and technical interoperability can help Ash identify and process the missing observations consistently, but they cannot determine whether the resulting sample is scientifically adequate for training an extreme-event prediction model.

Before calling the derived dataset AI-ready for this experiment, Ash therefore needs both interoperability and task-specific preparation. She needs a consistent representation, sufficient semantic information, programmatic access to the required source data, documented transformations and versions, and an explicit assessment of whether the resulting observations are appropriate for the prediction task.

The exercise demonstrates why AI readiness cannot be reduced to one format or infrastructure technology. It emerges from the combination of interoperable source data, reproducible processing, and evidence that the resulting training data are suitable for the intended task.

::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::: keypoints

AI readiness is task-dependent. No single data format or technology automatically makes a dataset AI-ready.

AI does not introduce a new interoperability layer. Instead, automated AI workflows increase the importance of structural, semantic, and technical interoperability.

Interoperable source data provide a foundation from which task-specific AI training datasets can be constructed reproducibly.

Cloud-native layouts such as Zarr can improve selective and parallel access for suitable large-scale workflows, but they do not automatically improve semantic interoperability or data quality.

Persistent identification, provenance, and versioning complement interoperability by connecting models and derived training data to the exact source data and transformations from which they were produced.

Interoperability supports reproducible and inspectable AI workflows, but it does not guarantee that training data are representative, scientifically appropriate, unbiased, or sufficient for a particular model.

::::::::::::::::::::
