## UAS Data Specification

The following provides the fields for the UAS data types. All data needs to be in JSON with location data encoded in [GeoJSON](http://geojson.org/). 

### UAS Data Types
The fields marked with an asterisk are already populated when you create the data source for your drone in [stae](https://stae.co). 

| Field | Data type | Description | Validation | Example
| ---   | --- 		| ---         | ---		   | ---
|id*    | Text      | Represents an unmanned aircraft system (UAV/UAS/Drone). | Not empty | "UAS"
|name*  | Text      | Descriptive name given to the UAS by the operator. | Not empty | "Drone Demo Test"
|type   | Text      | Categorization of the UAS. |  Not empty, max character length: 2048 | "Delivery Drone"
|notes* | Text 		| Description or further notes about the UAS. | Not empty | "Initial pilot project for Jersey City Deliveries"
|manufacturer| Text | Make/manufacturer of the UAS | Not empty, max character length: 2048 | "Parrot"
|model  | Text 		| Model of the UAS. | Not empty, max character length: 2048 | "AR Drone 2.0"
|color  | Text 		| Color of the UAS exterior. | Not empty, max character length: 2048 | "Gray"
|operators| Array 	| List of agencies of people responsible for the UAS. | Not empty, max character length: 2048 | ["Amazon, FAA"]
|active | Boolean 	| True/false if the UAS is currently in service. | - | True
|heading | Number (degrees) 	| Direction the UAS is pointing | Min: 0, Max: 360 | "175.45"
|speed 	| Number (KM/H) | Speed the UAS is currently going. | Min: 0, Max: 1000 | "5 KM/H"
|tripId | Text 		| Unique ID for the current trip the UAS is on. | Not empty, max character length: 2048 | "Riverview Park Delivery 01" 
|stream | Text 		| URL of the live video feed for the camera. | Not empty, max character length: 2048, stream URL exists | https://stae.co/dronefeed
|images | Array 	| List of images related to the UAS | Not empty, max character length: 2048 | [https://stae.co/drone1.jpg, https://stae.co/drone2.jpg]
|location | Point 	| Location where the UAS exists. | GEOJSON format | {"type": "Point", "coordinates": [-74.0429, 40.744]}
