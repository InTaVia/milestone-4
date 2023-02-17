# InTaVia Milestone-4

InTaVia Milestone 4 Documentation - Operational System 

System fully integrated, all main functionality implemented ready for testing by WP7.

# `Please Update Below`

## Backend (WP2 & 3)

### Data model InTaVia IDM-RDF

The repository https://github.com/InTaVia/idm-rdf contains the [IDM-RDF](https://github.com/InTaVia/idm-rdf/blob/main/idm-OWL/intavia_idm1.1.owl) at its current state (including ongoing discussions in the issues).

### Ingestion workflows / source datasets

The repository https://github.com/InTaVia/source-dataset-conversion contains the conversion scripts and the datasets in the IDM data model at their current state for the following prosopographical source datasets:

- [Finland](https://github.com/InTaVia/source-dataset-conversion/blob/main/BS_dataset/bs2intavia.ttl) (Aalto / UH)
- [Austria](https://github.com/InTaVia/source-dataset-conversion/blob/main/APIS_dataset/apis_oebl_serialization_2-9-2022.ttl) (OeAW)
- Netherlands [Part1](https://github.com/InTaVia/source-dataset-conversion/blob/main/intavia_biographynet/data/rdf/xaa.zip), [Part2](https://github.com/InTaVia/source-dataset-conversion/blob/main/intavia_biographynet/data/rdf/xab.zip), [Part3](https://github.com/InTaVia/source-dataset-conversion/blob/main/intavia_biographynet/data/rdf/xac.zip) (zipped in three parts for size) (VU)
- Slovenia [example subset](https://github.com/InTaVia/source-dataset-conversion/blob/main/SBI_dataset/sbi.ttl) (ZRC SAZU)

### Triplestore

The datasets are stored in a [Blazegraph](https://blazegraph.com/) triplestore, hosted at the ACDH-CH. Blazegraph provides a SPARQL endpoint for accessing the data.

### JSON API

The repository https://github.com/InTaVia/InTaVia-Backend contains an [OpenAPI](https://swagger.io/specification/) compliant [FastAPI](https://fastapi.tiangolo.com/)-based REST JSON API definitions and implementation for accessing the data in the InTaVia triplestore, at its current state.

Namely The following APIs are provided:

* Query Entities
* Retrieve Entity
* several vocabularies endpoints
* several statistics endpoints
* Reconciliation Service (implementing the [Reconciliation Service API
](https://reconciliation-api.github.io/specs/))

The test version of the API is available under https://intavia-backend.acdh-dev.oeaw.ac.at/v1/

### ResearchSpace for internal usage

In addition to the resources mentioned above we set up a [ResearchSpace instance](https://mp-playground.acdh-dev.oeaw.ac.at/) that is meant for internal work on and exploration of the knowledge graph. This service is currently for internal use only.

### Prefect workflow component

We deployed [Prefect](https://www.prefect.io/) within the  kubernetes cluster to run conversion, enrichment and ingestion scripts on the cluster. Given that the open-source version of this software solution doesnt come with authentication built in, this component is currently only reachable from within ACDH-CH subnet. Given some delay in our original planning the scripts are currently still executed locally instead of running within prefect.

The repository https://github.com/InTaVia/prefect-flows contains implementations for the the following workflows:

* Ingest data
* Entity ID linker
* Reconcile person instances
* Inference
* Enrich CHO data
* Convert GeoSpatial coordinates

## WP4

### NLP Abbreviations
We include the code to train an *abbreviation identification* classifier and to run an *abbreviation expansion* generator, both based on pre-trained transformer language models. The code can be found [here](https://github.com/InTaVia/nlp-abbreviations/releases/tag/v0.1). This is the preliminary version, where we test the concept on a small gold-standard slovene dataset (for now, a separate request for the dataset is required). More thorough evaluation and expansion of the experiments to Dutch and German coming soon.

### NLP Visualization
The milestone 3 version of our interactive text mining environment can be found [here](https://github.com/InTaVia/Performancer_AnnoXplorer/releases/tag/v1.0.0). It consists of two connected components, *Performancer* and *AnnoXplorer*, providing an overview and detail view on the data respectively; brushing over texts in the former displays them in the latter. 


## Frontend (WP5 & WP6)

The Milestone 4 Prototype (v0.2.0) of the InTaVia web client (frontend) is available as a permanent release: [https://github.com/InTaVia/web/releases/tag/v0.2.0](https://github.com/InTaVia/web/releases/tag/v0.2.0).

The current prototype is available online: [https://intavia.acdh-dev.oeaw.ac.at/](https://intavia.acdh-dev.oeaw.ac.at/).

*Data:* The connection between the backend and the frontend is now established replacing the initially used mock data. As additional data source, users can import their own local data into the application using a excel template.

The prototype implements functionalities used across the three top-level components (Data Curation Lab (DCL), isual Analytics Studio (VAS), and Storytelling Suite (STS) = Story Creator (SC) + Story Viewer (SV)) in a single application (except the SV, which is a seperate application). The functionalities implemented are:

### State managment and normalized entity cache
- **Shared Redux store** managing global sate that can be consumed by all components. For example, visualizations created in the VAS can be reused in the SC because visualizations are put into the global state.
- **Shared frontend network module** using RTKQuery handling deduplication of requests to data endpoints and caching of results. Entities and events are cached by id, to avoid repeated requesting and caching.

### Loading and fetching data
- **Typed API client** providing data fetching functions (towards the backend = InTaVia JSON API) and defining types of query parameters and types of the response shape (InTaVia JSON data model). The client also provides a zod validation schema to validate responses. The api-client is managed in a seperate [github repository](https://github.com/InTaVia/api-client) and is provided as [npm package](https://www.npmjs.com/package/@intavia/api-client).
- **Text-based queries** on entiy labels and list view of search results in the DCL.
- **Visual queries** are a visual way to formulate a search for person entities based on attribute constraints including name, date of birth, date of death, and occupation.
- **Custom react hooks** fetch missing data on demand when required by ui components depicting the data in some form (i.e., data panel, visualizations, detail view).
- **Local data import** provides functions to load data from an Excel document (e.g., data about [Albrecht Dürer](https://github.com/InTaVia/data-import/raw/main/public/data/data-duerer.xlsx)) with a predifined strucutre replicating in large parts the frontend data model.The data import is managed in a seperate [github repository](https://github.com/InTaVia/data-import) and is provided as [npm package](https://www.npmjs.com/package/@intavia/data-import).

### Data curation/editing
- **Collections** allow users to group entities and events by any topic of interest. Collections are created in the DCL and can then be used in the VAS and the SC to access stored data and visualize them with the help of the data panel. 

### Data views
- **Entity details** are shown on an entities detail view, listing attributes and events as well as displaying a ego-network visualization of the selected entity.
- **Data panel** provides a text or list-based view onto the data. The data panel is used in the VAS and the SC as a link to the redux data cache (i.e., collections and the respective events and entities). Users can interacively add data listed to visualizations.

### Visualizations
- **Timeline** view implements a component that visually adapts to the number of displayed entities and events and the available display space. To avoid overplotting several aggregation modes were developed.
- **Geographic map** view implements map layers that (1) are plotting simple markers at a given event location, (2) cluster proximal data points in a visually aggregated form, and (3) connect event locations chronologically with trajectories/lines.
- **Ego-Network** is currently used on the entity detail screen, showing related entities of the selected focus entity.
- **Statistical views** are currently used in the visual querying tool (i.e., historgram)
- **Visualization wizard** is shown in empty layout panels in the VAS and SC and allows to add new visualizations or load already existing visualizations into the respective panel.

### Layouts and coordinated views
- **Workspaces** in the VAS can have several predefined layouts containing up to four visualizations implementing multiple views
- **Slides** in the SC can have have several predefined layouts containing visualizations and narrative content

### Story creation

- The ST creator implements an interactive user interface allowing to generate and slide-based stories. The prototype can depict entities and their events on maps and timelines and provides  (i.e., images, text, quizzes): [https://intavia.acdh-dev.oeaw.ac.at/storycreator](https://intavia.acdh-dev.oeaw.ac.at/storycreator) 
    - Stories Overview (create and delete stories, edit story settings)
    - Story Flow (create, duplicate, delete, and order slides per drag and drop)
    - Slide Editor (create, edit, select-annotate, layout and delete visualizatiions as well as narrative content including text, images, and quizzes)
    - Story script export (to the SV)

### Story viewing
- An example of a story created using the Story Creator is available online: 
[Story Viewer Example](https://intavia.fluxguide.com/fluxguide/public/content/fluxguide/exhibitions/1/system/app/dist/index.html)
    - The example shows 4 Slides of the **"Albrecht Dürer's trip to the Netherlands"** created with the **Story Creator** based on a map visualization featuring annotations and images.
