# Layer Metadata Server Object Extension #

This project is an ArcGIS Server Object Extension (SOE) that allows a map service to publish the metadata for its individual layers to the user.

## License ##
This program is licensed under the [MIT license](http://www.opensource.org/licenses/MIT)

## Prerequisites ##

* ArcGIS Server 10.0.  (Not yet tested with ArcGIS Server 10.1.  Since the SOE registration process was completely changed between 10.0 and 10.1, it is very possible that this SOE will not be compatible with 10.1.)
* Microsoft .NET Framework 3.5

## Compilation ##

### Requirements ###

* Microsoft Visual Studio 2010.
* ArcGIS ArcObjects SDK (for ArcGIS version 10 or higher).

### Instructions ###

1. Open the solution file in Visual Studio 2010.
2. Build the solution.  The compiled output for the SOE and installation program will be written to the bin folder.

## Usage ##

### validLayers ###
Retrieves a an array of of layer ids.

http://*YourServer*/ArcGIS/rest/services/*YourMap*/MapServer/exts/LayerMetadata/validLayers?f=__format__

#### Parameters ####
* f: Options are `json` and `pjson`.

### layerSources ###
Retrieves a grouped list of layer IDs.  The output will have one property corresponding to each unique data source (feature class).  The value of each property is an array of integers corresponding to all of the layer IDs that share that data source.

http://*YourServer*/ArcGIS/rest/services/Shared/*YourMap*/MapServer/exts/LayerMetadata/layerSources?f=__format__

#### Parameters ####
* f: Options are `json` and `pjson`.

### metadata ###

Retrieves the metadata document for the specified layer.

http://*YourServer*/ArcGis/rest/services/*YourMap*/MapServer/exts/LayerMetadata/metadata/__layerId__/?f=__format__

#### Parameters ####
* layerId: An integer corresponding to a feature layer ID in the map service.
* f: The output format.  Valid options are `xml` and `html`.

### getMetadata ###
Retrieves the metadata document for the specified layer.

http://*YourServer*/ArcGIS/rest/services/*YourMap*/MapServer/exts/LayerMetadata/getMetadata?layer=__layer__&f=__format__

#### Parameters ####
* layer: An integer corresponding to a feature layer ID in the map service.
* f: The output format.  Valid options are `xml` and `html`.

## Projects ##

### LayerMetadata ###

This project is the actual Server Object Extension (SOE).

### RegisterSOE ###

This program is used to register an SOE with ArcGIS Server.  It can work with any ArcGIS Server SOE assembly provided you change the application configuration file.

### LayerMetadataClient ###

This project contains extensions to the [ArcGIS JavaScript API](http://links.esri.com/javascript) layer classes that allow them to call the Layer Metadata SOE.

#### metadataExtensions.js ####

This JavaScript file extends the layer classes to add methods for calling the Layer Metadata SOE.

#### unittest.html ####

QUnit unit test for metadataExtensions.


##### Unit Test Setup #####

1. Copy `unittest.js.sample` to `unittest.js`.  (`unittest.js` is in the `.gitignore` file, so it will not be pushed to the repository.)
2. Modify the url variable in `unittest.js` so that it points to a map service that has the _Layer Metadata_ capability enabled.


## Troubleshooting ##

### I have registered the SOE, but it is not showing up in the list of capabilities in ArcGIS Server Manager.  It does show up in ArcCatalog, though. ###

Restart IIS on the ArcGIS Server.


## F. A. Q. ##

### ArcGIS Server 10.1 has metadata capabilities.  Aren't you wasting your time writing this? ###

ArcGIS Server 10.1 server only adds the ability to publish a single metadata document per map service.

What this SOE will do is pubish one metadata document for each LAYER in a map service.