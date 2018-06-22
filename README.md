### Developer Documentation
Below is a walkthrough with code samples on setting up a delivery drone registry and regulations in Stae. We'll start by looking up a city's existing drone registry. Then, we'll submit our drone to the city's registry and create a proposed delivery route. Next, we'll go over how cities can use Stae to develop a basic regulatory framework, consisting of altitude checks, airpsace usage fees, and designating the responsible agencies. Finally, we'll create an emergency landing zone for drone operators to use. 

#### Data Type look up

```java
import request from 'superagent'

request
  .get('https://municipal.systems/v1/municipalities/municipalityId/dataTypes/uas/data')
  .query({ key: 'your personal api key!' })
  .end((err, res) => {
    console.log(res.body)
  })
```

This returns a city's current drone registry. To find your city's municipalityId, edit the get request to ```'https://municipal.systems/v1/municipalities'``` to return a list of municipalities. Your personal api key is located on the **My Account** page.

#### Sample code registers a drone into the city's drone registry. This code requires that UAS data source is created first along with a valid api key.

```java
import request from 'superagent'

request
  .post('https://municipal.systems/v1/data')
  .query({ key: 'your source api key!' })
  .send({
    name: 'Central Business District Delivery Drone',
    id: '54321',
    type: 'delivery drone',
    manufacturer: 'Parrot',
    model: 'AR Drone 2.0',
    color: 'Gray',
    active: false,
    images: [
    		"https://stae.co/drone1.jpg", 
    		"https://stae.co/drone2.jpg"
    		],
    speed: 0
    heading: 0
    tripId: 'Waterfront Delivery 01'
    operators: [
    			"Department of Transportation", 
    			"Department of Innovation"
    			],
    notes: 'Initial pilot project for CBD deliveries',

    location: {
        "type": "Point",
        "coordinates": [
          -74.0374419093132,
          40.72505534660708
          ]
      }
  })
  .end((err, res) => {
    console.log(res.body)
  })
```

#### Sample code proposes a UAS flight path, start and end dates, along with a payment ID for any fees paid to perform this trip. This code requires that a UAS Trip data source is created along with a valid api key. 

```java
import request from 'superagent'

request
  .post('https://municipal.systems/v1/data')
  .query({ key: 'your source api key!' })
  .send({
    start: 07/01/2018 12:00 PM,
    end: 07/01/2018 01:00 PM,
    name: 'Central Business District Delivery Drone',
    id: '98765',
    type: 'delivery drone',
    tripId: 'Waterfront Delivery 01',
    paymentId: '123321',
    images: [
    		"https://stae.co/drone1.jpg", 
    		"https://stae.co/drone2.jpg"
    		],
    operators: [
    		"Department of Transportation", 
    		"Department of Innovation"
    		],
    notes: 'Proposed delivery route for CBD drone',

    location: {
        "type": "LineString",
        "coordinates": [
          [
            -74.0375304222107,
            40.725039084927516
          ],
          [
            -74.03405427932739,
            40.72886860074364
          ],
          [
            -74.03244495391846,
            40.72791734031611
          ],
          [
            -74.03323888778687,
            40.727144939367555
          ],
          [
            -74.0349018573761,
            40.72473824313938
          ],
          [
            -74.03723001480101,
            40.7250472157678
          ]
        ]
      }
  })
  .end((err, res) => {
    console.log(res.body)
  })
```


#### Sample code designates a UAS restriction zone, lists responsible agencies, and fees for airspace usage. This code requires that a UAS Restriction data source be created first along with a valid api key.

```java
import request from 'superagent'

request
  .post('https://municipal.systems/v1/data')
  .query({ key: 'your source api key!' })
  .send({
    name: 'Downtown Delivery Drone',
    id: '54321',
    type: 'delivery drone',
    manufacturer: '1',
    ceiling: '121',
    fee: '100',
    operators: [
    		"Department of Transportation", 
    		"Department of Innovation"
    		],
    notes: 'Initial pilot project for Jersey City deliveries',

    location: {
    	"type": "Polygon",
        "coordinates": [
          [
            [
              -74.04982566833496,
              40.72966537251703
            ],
            [
              -74.05068397521973,
              40.72420160305987
            ],
            [
              -74.05102729797363,
              40.722380246890005
            ],
            [
              -74.05128479003906,
              40.72198994979771
            ],
            [
              -74.0412425994873,
              40.718997596057285
            ],
            [
              -74.03986930847167,
              40.72868973229961
            ],
            [
              -74.04982566833496,
              40.72966537251703
            ]
          ]
        ]
      }
  })
  .end((err, res) => {
    console.log(res.body)
  })
```  

#### Sample code designates an emergency landing zone where a UAS can make emergency landings. This code requires that a UAS emergency landing zone data source be created first along with a valid api key.

```java
import request from 'superagent'

request
  .post('https://municipal.systems/v1/data')
  .query({ key: 'your source api key!' })
  .send({
    name: 'Emergency Drone Landing Zone',
    id: '56789',
    type: 'parking lot',
    operators: [ 
    		"Department of Emergency Management", 
    		"Department of Innovation"
    		],
    notes: 'Emergency landing zone on top floor of parking garage.',
    images: '[https://stae.co/parking-garage.jpg]',

    location: {
    	"type": "Polygon",
        "coordinates": [
          [
            [
              -74.03744459152222,
              40.72579321613555
            ],
            [
              -74.03758943080902,
              40.72496184189546
            ],
            [
              -74.037044942379,
              40.72491508949041
            ],
            [
              -74.03649508953094,
              40.72535415426129
            ],
            [
              -74.03643608093262,
              40.725711908599614
            ],
            [
              -74.03744459152222,
              40.72579321613555
            ]
          ]
        ]
      }
  })
  .end((err, res) => {
    console.log(res.body)
  })
```

## UAS Data Specification

The following provides the fields for the UAS data types. All data needs to be in JSON with location data encoded in [GeoJSON](http://geojson.org/). 

### UAS Data Types
The fields marked with an asterisk are already populated when you create the data source for your drone in stae. 

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
