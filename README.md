# InTaVia Milestone-4

InTaVia Milestone 4 Documentation - Basic Operational System 

Individual components of the system operational with connectivity partially established and ready for evaluation by WP7.


## Backend (WP2 & 3)

### Datamodel InTaVia IDM-RDF

The repository https://github.com/InTaVia/idm-rdf contains the [IDM-RDF](https://raw.githubusercontent.com/InTaVia/idm-rdf/main/idm-OWL/intavia_idm1.owl) at its current state (including ongoing discussions in the issues).

### Ingestion workflows / source datasets

The repository https://github.com/InTaVia/source-dataset-conversion contains the conversion scripts and the datasets in the IDM data model at their current state for the following prosopographical source datasets:

- [Finland](https://raw.githubusercontent.com/InTaVia/source-dataset-conversion/main/BS_dataset/bs2intavia.ttl) (Aalto / UH)
- [Austria](https://raw.githubusercontent.com/InTaVia/source-dataset-conversion/main/APIS_dataset/apisdata_18-04-2022_edited.ttl) (OeAW)
- Netherlands [Part1](https://raw.githubusercontent.com/InTaVia/source-dataset-conversion/main/intavia_biographynet/data/rdf/xaa.zip), [Part2](https://raw.githubusercontent.com/InTaVia/source-dataset-conversion/main/intavia_biographynet/data/rdf/xab.zip), [Part3](https://raw.githubusercontent.com/InTaVia/source-dataset-conversion/main/intavia_biographynet/data/rdf/xab.zip) (zipped in three parts for size) (VU)

### JSON API

The repository https://github.com/InTaVia/grlc contains the grlc software, that is used for providing the JSON API, at its current state.

The repository https://github.com/InTaVia/grlc_sparql contains the JSON API definitions at their current state, namely the following APIs:

* Generic entity search
* Person history

The test version of the API is available under https://grlc.acdh-dev.oeaw.ac.at/api/InTaVia/grlc_sparql#/


### Researchspace for internal usage

In addition to the resources mentioned above we set up a [researchspace instance](https://mp-playground.acdh-dev.oeaw.ac.at/) that is meant for internal work on and exploration of the knowledge graph. This service is currently for internal use only.

### Prefect workflow component

We deployed prefect within the ACDH-CH kubernetes cluster to run conversion, enrichment and ingestion scripts on the cluster. Given that the open-source version of this software solution doesnt come with authentication built in, this component is currently only reachable from within ACDH-CH subnet. Given some delay in our original planning the scripts are currently still executed locally instead of running within prefect. 


## WP4

### NLP Abbreviations
We include the code to train an *abbreviation identification* classifier and to run an *abbreviation expansion* generator, both based on pre-trained transformer language models. The code can be found [here](https://github.com/InTaVia/nlp-abbreviations/releases/tag/v0.1). This is the preliminary version, where we test the concept on a small gold-standard slovene dataset (for now, a separate request for the dataset is required). More thorough evaluation and expansion of the experiments to Dutch and German coming soon.

### NLP Visualization
The milestone 3 version of our interactive text mining environment can be found [here](https://github.com/InTaVia/Performancer_AnnoXplorer/releases/tag/v1.0.0). It consists of two connected components, *Performancer* and *AnnoXplorer*, providing an overview and detail view on the data respectively; brushing over texts in the former displays them in the latter. 


## Frontend (WP5 & WP6)

The Milestone 3 Prototype (v0.1.0) of the InTaVia web client (frontend) is available as a permanent release: [https://github.com/InTaVia/web/releases/tag/v0.1.0](https://github.com/InTaVia/web/releases/tag/v0.1.0).

The current prototype is available online: [https://intavia.acdh-dev.oeaw.ac.at/](https://intavia.acdh-dev.oeaw.ac.at/).

The data shown in the application are automatically generated mock data including person and place entities. As soon as the connection between the backend and the frontend is established, the mock data will be replaced. **Note:** if below linked visualizations do not show results, please first do a search on the search page and then switch back to the visualization.

The prototype implements aspects of the three top-level components (Data Curation Lab, DC lab; Visual Analytics Studio, VA studio; Visual Storytelling Suite, ST suite) in a single application on separate pages. The components implemented are:

### DC lab

- Keyword search and list view of search results: [https://intavia.acdh-dev.oeaw.ac.at/search](https://intavia.acdh-dev.oeaw.ac.at/search)
- Detail view of entities: e.g., [https://intavia.acdh-dev.oeaw.ac.at/person/876859d3-dee8-468d-9c61-a29e97ef478a](https://intavia.acdh-dev.oeaw.ac.at/person/876859d3-dee8-468d-9c61-a29e97ef478a)
- Basic editing capability of person entities (name, description, event types & dates) - click edit on the detail view page.
- Shared global Redux store that can be accessed by all components 
- Shared frontend network module using RTKQuery (handles deduplication of requests to data endpoints and caching of results)

### VA studio

Data views:
- Timeline view showing a set of person entities with selected life events (e.g. birth, death, lived): [https://intavia.acdh-dev.oeaw.ac.at/timeline](https://intavia.acdh-dev.oeaw.ac.at/timeline)
- A geographic map view showing localized life events (i.e. birth and death) connected with a line: [https://intavia.acdh-dev.oeaw.ac.at/geomap](https://intavia.acdh-dev.oeaw.ac.at/geomap)

Coordinated views:
- Multiple views integrated on a single page are shown here: [https://intavia.acdh-dev.oeaw.ac.at/coordination](https://intavia.acdh-dev.oeaw.ac.at/coordination)
- The example coordinates an entity list view, a timeline and a map showing persons and their life events via mouseover highlighting (red colour).
- Currently, applies only to the map, a dropdown menu allows to filter the depicted events. If more than one event type is selected, the localized events are connected with lines in chronological order.

Visual querying:
- A workspace to visually query for persons based on attribute constraints including name and date of birth and death: [https://intavia.acdh-dev.oeaw.ac.at/visual-querying](https://intavia.acdh-dev.oeaw.ac.at/visual-querying)

### ST suite = ST creator + ST viewer

- The ST creator implements an interactive user interface allowing to generate and import slide-based stories. The prototype can depict persons’ life-events on a map and provides annotation capabilities (i.e., images and text): [https://intavia.acdh-dev.oeaw.ac.at/storycreator](https://intavia.acdh-dev.oeaw.ac.at/storycreator) 
    - Stories Overview (create and delete stories)
    - Story Flow (create, layout and delete slides per drag and drop)
    - Slide Editor (create, edit, layout and delete map, images and text per drag and drop)
    - Text Mode (edit or upload the whole story via textarea) (accessible through the clipboard icon in the top right corner)

- The single event slides for the **"Life of Paolo Vergerio"** can be imported through the text editor using the content of json this [file](https://drive.google.com/file/d/1SSqNUxFXwucLqy2qHig4TAgyPweHNRJd/view?usp=sharing).

- A clickable Adobe XD prototype of the ST viewer is available online: [ST Viewer Prototype](https://xd.adobe.com/view/b3de8bf3-1e43-434a-b06f-a99939eb2e6e-e68d)
    - The prototype tells the story **"Life of Paolo Vergerio"** primarily based on a map/timeline visualization featuring annotations, images, quiz and timebased visualizations. 
    - All screens including a selection of desktop views are available as PNG: [Screens ST viewer [.zip]](https://www.dropbox.com/sh/nvn626iro0yz1g7/AAD8mhhJHn79GT3mC-C-VNxaa?dl=1)
    
