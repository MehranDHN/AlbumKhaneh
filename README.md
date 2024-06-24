# AlbumKhane

## Overview

This project aims to integrate approximately 3000 photographs hosted in an **IIIF-compliant** manner on the **Internet Archive** compatible with **RDF** (Resource Description Framework) and graph data technologies.By leveraging *RDF* and common vocabularies/Ontology providers such as **WikiData** and **DBPedia** we want to enhance the discoverability and interconnectivity of the photographic collection, enabling rich semantic queries and advanced data relationships. This is our priority, however, thanks to IIIF 3.0, the presentation quality improved vastly as we will describe later. 

<img src = "https://ids.si.edu/ids/iiif/FS-FSA_A.4_2.12.GN.23.07/full/full/0/default.jpg" alt="">

## Table of Contents

- [Project Overview](#overview)
- [Features](#features)
- [Project Logical Organization](#logicalorganization)
- [Machine Readable Data](#machinereadabledata)-
- [Targets](#targets)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Data Model](#data-model)
- [IIIF Integration](#iiif-integration)
- [RDF and Graph Data](#rdf-and-graph-data)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Features

- IIIF Compliant: All photographs are hosted on the Internet Archive and are automatically  IIIF-compliant, ensuring high-quality image delivery and compatibility with IIIF viewers.
- RDF Integration: Photographs are described using RDF, allowing for semantic annotation and linking.
- Graph Database: A graph database is used to store and query the RDF data, enabling complex queries and data relationships.
- Semantic Search: Advanced search capabilities based on RDF and graph data.
- Interoperability: Supports integration with other IIIF and RDF-based systems.

## Project Logical Organization

The project has a one-to-one map between each folder/album and a logical container in the Internet Archive.
Based on the original naming convention we assume that each album should have a four-digit numeric code from 0100 to 1429 but not sequentially because there are many gaps between them and we don't know why.
In this project, every Photograph has it's own unique code starting from a static shared **GPAK** prefix followed with a unique **Album Code**.
We also assume that each album contains less than 100 photographs so the photograph code is always a zero-padded two-digit number.
We use a dash[-] character to separate  the **Prefix**, **Album Code**, and sequential *Photograph Code* as **GPAK**-**AAAA**-**nn**.
This rule has an exception that we will refer to it later.
By this naming convention, each photograph has a unique code that enables us to logically group  the photographs together as an album, thanks to the tagging system in **Internet Archive** by using the **Genre** metadata key which is a Taxonomy/Clickable field.
- Album **GPAK-0100** 
- - <a href="https://archive.org/search?query=genre%3A%22GPAK-0100%22&sort=title" target="_blank"> GPAK-0100 album sorted by title.</a>
- Album **GPAK-0108**
- - <a href="https://archive.org/search?query=genre%3A%22GPAK-0108%22&sort=title" target="_blank"> GPAK-0108 album sorted by title.</a>- 
- Album **GPAK-0114**
- - <a href="https://archive.org/search?query=genre%3A%22GPAK-0114%22&sort=title" target="_blank"> GPAK-0114 album sorted by title.</a>
- Album **GPAK-0117**
- - <a href="https://archive.org/search?query=genre%3A%22GPAK-0117%22&sort=title" target="_blank"> GPAK-0117 album sorted by title.</a>               

## Machine Readable Data

When we upload an image to Internet Archive, its corresponding IIIF Metadata is also generated as machine-readable data suitable for computers to read/parse and whatever they want to do with that data.
The URL for accessing The IIIF manifest of each photograph is generated based on the following pattern:<br>
 https://iiif.archive.org/iiif/3/Photograph_Code/manifest.json<br>
 So the IIIF metadata of the 5th photograph in second album with the GPAK-0108-05 can be assessed via  https://iiif.archive.org/iiif/3/GPAK-0108-05/manifest.json<br/>
 
However, the Internet Archive has a predefined endpoint for getting the full metadata of any item that can be reached from :<br>
https://archive.org/metadata/Photograph_Code/

## Targets

Besides the main target of the **AlbumKhaneh** project which is integrating all the newly exposed Golestan Palace photographs in **IIIF-compliant collections** and relating them to **RDF-based semantic Web** authorities like WIikiData, this project aims to enhance Wikidata info by using the tags and metadata collected from the **AlbumKhaneh** project and improve the implicit and explicit relations between the entities. This will be achieved by designing and implementing a specialized service that processes inputs from at least two main groups. The inputs are matched with existing Wikidata entries, and those not present will be added as new entries.

The first logical group is political figures and celebrities, and the second group includes cities, buildings, and historical places.<br>
Before and after any actions, statistics related to the current situation and the details about any progress will be prepared and presented, however real-time monitoring of the quality of data will be available through **SPARQL**.

Below is an image of a Wikidata entry for [(https://wikidata.metaphacts.com/resource/wd:Q5708260)]
Aqa Ebrahim Amin-ol-Soltan was a famous politician of Iran in the Qajar era.<be>

<img src="https://github.com/MehranDHN/AlbumKhaneh/blob/main/GQ04xEvWIAA8ZRe.jpg" alt="WikiData: A Human Data Model">

If we want to get a list of Politicians who lived in the 18th and 19th centuries we can submit the following **SPARQL** query :<br>

```turtle
SELECT ?item ?itemLabel ?birth
WHERE
{
  ?item wdt:P31 wd:Q5;        # Must be a human
        wdt:P27 wd:Q794;      # citizenship IRAN
        wdt:P106 wd:Q82955.   # occupation Politician
        ?item wdt:P569 ?birth.# transfering birthdate in new variable
    # Filter the result for those whose birth date was before the start of the 20th century         
    FILTER(
       ((year(?birth)< 1900) && (year(?birth)> 1700))
     )
  SERVICE wikibase:label { bd:serviceParam wikibase:language "fa,en". } # our label priority is persian and then english
}
```

## Architecture

The project architecture consists of the following components:

1. IIIF Image Hosting: Photographs are hosted on the Internet Archive, which provides IIIF 3.0-compliant image services.
2. RDF Data Store: Each Agential, Temporal, and Spatial Data in the photographs are interconnected with related entries in WikiData but right now they are at a minimum level and extra work is needed to create a useful Knowledge Graph for each album and then to merge them in a single independent Graph Knowledge.
We intend to implement a special normalizer to normalize the IIIF 3.0 Manifest of each image and create proper RDF statements from their metadata.
3. Graph Database: Used to store and query RDF data, supporting complex relationships and queries.
4. Web Interface: A user-friendly web interface for searching and viewing photographs.

## Installation

To set up the project locally, follow these steps:

1. Clone the Repository:
   
    git clone https://github.com/MehranDHN/AlbumKhaneh.git
    cd AlbumKhaneh
    
2. Install Dependencies:
   
    pip install -r requirements.txt
    
3. Set Up the Graph Database:
    - Install and configure your preferred graph database (e.g., Neo4j, Blazegraph).
    - Load the RDF data into the graph database.

4. Configure IIIF Integration:
    - Ensure the IIIF image URLs from the Internet Archive are correctly referenced in the RDF data.

## Usage

1. Start the Web Server:
   
    python app.py
    
2. Access the Web Interface:
    Open a web browser and navigate to http://localhost:5000 to start exploring the photograph collection.

## Data Model

The RDF data model includes the following key entities:

- Photograph: Represents an individual photograph with properties such as title, description, creator, date, and IIIF URL.
- Collection: Represents a collection of photographs.
- Person: Represents individuals associated with the photographs (e.g., photographers, subjects).
- Event: Represents events depicted in or related to the photographs.

## IIIF Integration

The photographs are hosted on the Internet Archive, which provides IIIF-compliant image services. Each photograph's RDF description includes the IIIF URL, enabling high-quality image delivery and interaction.

## RDF and Graph Data

The project uses RDF to describe the photographs and their relationships. RDF data is stored in a graph database, enabling complex semantic queries and data interlinking. SPARQL is used for querying the RDF data.

## Contributing

We welcome contributions from the community. To contribute, please fork the repository and submit a pull request with your changes. Ensure that your code adheres to the project's coding standards and includes appropriate tests.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Internet Archive: For hosting the photographs and providing IIIF image services.
- IIIF Community: For the standards and tools that make interoperable image sharing possible.
- - RDF and Graph Database Communities: For the frameworks and technologies that enable rich semantic data integration.
