Data management workshop
========

LNCC/GBIF - August 25-29, 2014

Monday
--------

#### Introduction
* Overview of our software products, research areas, and metadata/data lifecycle: EML -> Morpho -> Metacat -> DataONE -> R client
* Importance of quality metadata for archiving data, now and for the future.

#### DataONE R client.
* Incorporating data and metadata available via the DataONE API in your R scripts.
* Automatic dataframe parsing for well-described tabular data.
* Saving your results back to the network (full life-cycle for synthetic data analysis!)

#### Hands-on: 
* Online data registry. Add sample data packages to a test Member Node
    + https://dev.nceas.ucsb.edu/#share
* Interact with Metacat search UI and DataONE to locate those test packages
    + MN: https://dev.nceas.ucsb.edu/#data
    + CN: https://cn-stage-2.test.dataone.org/onemercury/
* DOI publishing, citation, and resolution.
    + http://ezid.cdlib.org/lookup
		
#### Hands-on: 
* Installing DataONE R client and using data uploaded in prior sessions in simple analyses and visualizations.
		
Tuesday
--------
	
#### All about Morpho.
* Describing data packages, projects, collection methods, protocols, data table attributes, etc.
* Importing and exporting data and metadata, saving to repository, making revisions, etc

#### Hands-on: 
* Installing Morpho, troubleshooting installations, internationalization settings, etc.
    + https://knb.ecoinformatics.org/#tools/morpho
* Editing existing packages.
* Extending metadata for the packages added via online registry from the previous session.
* Working with the datapackage wizard to make new packages.
* Exploring other features.
	
Wednesday (short morning)
--------------------------

* EML internationalization techniques.
* Datapackage structures
    + EML packages
    + OAI-ORE resource maps
* Working with EML and the datamanager library: from quality control to data synthesis


Thursday
--------
	
#### All about Metacat
* Review the features and components
* Interacting with the user interface (UI)
    + https://knb.ecoinformatics.org/knb/docs/themes.html
* Using APIs to access, add, and modify content.
    + Metacat API (deprecated)
    + DataONE API
    + SOLR queries (metacat-index)

#### Hands-on: 
* Install Metacat UI
* Customize a UI theme
    + Documentation [here](https://knb.ecoinformatics.org/knb/docs/themes.html#creating-a-custom-theme)
* Locate metadata records about certain topics with a SOLR query
    + Like this [example](https://dev.nceas.ucsb.edu/knb/d1/mn/v1/query/solr/fl=id&q=formatType:METADATA+-obsoletedBy:*&fq=manaus&wt=xml)

Friday
--------

#### Installing Metacat
* Review the application structure and dependencies for deploying a Metacat repository

#### Hands-on: 
* Installing Metacat, including all dependencies
    + Download and follow along [here](https://knb.ecoinformatics.org/knb/docs/)
* Highly technical, probably use the entire morning for this
    + Perhaps do this small teams (2-3 people) rather than have each participant installing their own Metacat instance?
    + Testing the installation: add content, search for content, point UI to local installation	
	
