# AlbumKhane

## Overview

This project aims to integrate approximately 3000 photographs hosted in a IIIF-compliant manner on the Internet Archive with RDF (Resource Description Framework) and graph data technologies. By leveraging RDF and graph data, we want to enhance the discoverability and interconnectivity of the photographic collection, enabling rich semantic queries and advanced data relationships.

<img src = "https://ids.si.edu/ids/iiif/FS-FSA_A.4_2.12.GN.23.07/full/full/0/default.jpg" alt="">

## Table of Contents

- [Project Overview](#overview)
- [Features](#features)
- [Project Logical Organization](#logicalorganization)
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

The project has a one to one map between each folder/album and a logical container in Internet Archive.
Based on the original naming convention we assume that each album should have a four digits numeric code from 0100 to 1429 but not sequentially because there are many gaps between them and we don't now why.
In this project every Photograph has it's own unique code starting from static shared **GPAK** prefix following with unique **Album Code**.
We also assume that each album contains less than 100 photograph so the photograph code is always a zero padded two digit number.
We use a dash[**-**] character to seperate  the **Prefix**, **Album code** and sequential photograph code as **GPAK**-**AAAA**-**nnnn**.
This rule has an exception that we will refer to it later.
By this naming convention each photograp has a unique code that enables us to logically grouped  the photogethes together as an album, thanks to tagging system in **Internet Archive** by using the **Genre** metadata key which is a Taxonomy/Clickable field.
- Album **GPAK-0100** 
-- <a href="https://archive.org/search?query=genre%3A%22GPAK-0114%22&sort=title"> GPAK-0100 album sorted by title</a>
- Album **GPAK-0108** 
- Album **GPAK-0114**
- 
MORE About each Image URL and its xoresponding IIIF Manifest HERE

## Architecture

The project architecture consists of the following components:

1. IIIF Image Hosting: Photographs are hosted on the Internet Archive, which provides IIIF 3.0-compliant image services.
2. RDF Data Store: Eaxh Agential,Temporal and Spatial Data in each photograph interconnected with related entries in WikiData but they are at mimum level and extra works needed to create usefull Knowledge Graph for each album and then to merge them in a single independent Fraph Knowledge.
We intent to implement special normalizer to normalize the IIIF Manifest of each image and create proper RDF statements from their metadata.
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
