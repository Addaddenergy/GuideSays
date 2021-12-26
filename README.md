# `.stx` (soundtrail) Web Viewer


This is an open source, publicly available, really basic, .`stx` file viewer.

This version is released as-is as a starting point for the community.
PRs are welcome.

### Information about the `.stx` format

* [The `.stx` format definition](./stxFormatDefinition.md)

* [Article announcing the format](https://medium.com/@medi.muse/introducing-the-new-sound-trail-data-format-66a7d6454efc?source=friends_link&sk=20b65cec44a97735fefd2d6d4bce201e)

* [Article with initial thoughts leading to the concept](https://medium.com/@medi.muse/introducing-the-new-sound-trail-data-format-66a7d6454efc?source=friends_link&sk=20b65cec44a97735fefd2d6d4bce201e)

### To view the sample

You can view the sample `.stx` using

```
serve .
```

(install "serve" using `npm i -g serve`)

Then open http://localhost:5000


(or use the `run.bat` included for a python server)


### To try a different `.stx` with this viewer

To try your own `.stx` file in the viewer, currently you must simply change the line in `index.html` which is currently 

```
const soundtrail = "./zip/Nambour.stx"
```

to the URL of your own .stx file.


### TODO

* Need to remove soundtrails.com hardcoding
* Allow upload and viewing of an arbitrary `.stx` file    
   (or, at a minimum, add it as URL param)
* Add polygon support
* Multiple image support 


<!DOCTYPE html>
<html>

<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>.stx file viewer : Soundtrail demo with Leaflet.js</title>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <link rel="stylesheet" type="text/css" href="//cdn.jsdelivr.net/npm/slick-carousel@1.8.1/slick/slick.css" />
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
        integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
        crossorigin="" />
    <script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js"
        integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew=="
        crossorigin=""></script>
    <script type="text/javascript" src="http://code.jquery.com/jquery-3.4.1.min.js"></script>
    <script type="text/javascript" src="//cdn.jsdelivr.net/npm/slick-carousel@1.8.1/slick/slick.min.js"></script>
    <script src="SoundtrailParser.js"></script>
</head>

<body>
    <div id="map"></div>
    <style>
        audio::-webkit-media-controls-mute-button {
            display: none !important;
        }

        audio::-webkit-media-controls-volume-slider {
            display: none !important;
        }

        #map {
            height: 900px;
        }

        .leaflet-popup-content-wrapper,
        .leaflet-popup-tip {
            width: 440px;
            height: 640px;
        }
    </style>
    <script>
        const map = L.map('map').setView([-26.6237, 152.9588], 20);
        map.setMaxZoom(24)
        map.setMinZoom(18)
        const showMap = false // if you want to display the OSM layer
        if (showMap) {
            const osm = L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png');
            map.addLayer(osm);
        }
        const soundtrail = "./samples/Nambour.stx"
        // pick your soundtrail
        //const soundtrail = "https://ccpublicbucket.s3-ap-southeast-2.amazonaws.com/published_soundtrails/Nambour.soundtrail"

        // this'll take care of the rest
        fetch(soundtrail)
            .then(res => res.text())
            .then(kmltext => {
                // Create new kml overlay
                const parser = new DOMParser();
                const kml = parser.parseFromString(kmltext, 'text/xml');
                const track = new L.KML(kml);

                map.addLayer(track);
                // Adjust map to show the kml
                var bounds = track.getBounds();

                map.fitBounds(bounds); // zoom and centre map to fit the known hotspots

            });
    </script>
</body>

</html> 
python -m http.server 1337
<?xml version="1.0" encoding="UTF-8"?>
<kml xmlns="http://www.opengis.net/kml/2.2">
  <Document>

    <ExtendedData>
        <Data name="title">
            <value>Nambour Sound Walk</value>
        </Data>
        <Data name="desc">
            <value>Drawing together history and heritage, local stories and great original music, the Nambour Heritage Soundtrail is a contemporary take on a truly historical town.</value>
        </Data>
        <Data name="imageBaseUrl">
            <value>https://soundtrails.com.au/soundwalks/datafiles/images/</value>
        </Data>
        <Data name="images">
            <value>soundwalk_image_596457907caef, soundwalk_image_596457b1a0841, soundwalk_image_596457d396bce,
            soundwalk_image_59645808f1741, soundwalk_image_596458482974e, hotspot_image_59768cde8e57d</value>
        </Data>
    </ExtendedData>

    <Placemark id="336">
        <Point>
            <coordinates>152.9598328471183800,-26.6269910082151250</coordinates>
        </Point>
        <ExtendedData>
            <Data name="radius">
                <value>150</value>
            </Data>
            <Data name="audioFileUrl">
                <value>https://soundtrails.com.au/soundwalks/datafiles/sounds/sound_file_595082f26d231.mp3</value>
            </Data>
            <Data name="title">
                <value>Howard St and the Cane Train Haul</value>
            </Data>
            <Data name="desc">
                <value>Memories of the trains that went past and the chaos that sometime ensued.</value>
            </Data>
            <Data name="images">
                <value>hotspot_images_5959a829a004b, hotspot_images_5964336951ee9</value>
            </Data>
        </ExtendedData>
    </Placemark>

    <Placemark id="337">
        <Point>
            <coordinates>152.9598516225814800,-26.6256986176855720</coordinates>
        </Point>
        <ExtendedData>
            <Data name="radius">
                <value>520</value>
            </Data>
            <Data name="audioFileUrl">
                <value>https://soundtrails.com.au/soundwalks/datafiles/sounds/sound_file_59508441c1fb5.mp3</value>
            </Data>
            <Data name="title">
                <value>Jazzy got love - background</value>
            </Data>
        </ExtendedData>
    </Placemark>

    <Placemark id="338">
        <Point>
            <coordinates>152.9598516225814800,-26.6264083552957870</coordinates>
        </Point>
        <ExtendedData>
            <Data name="radius">
                <value>147</value>
            </Data>
            <Data name="audioFileUrl">
                <value>https://soundtrails.com.au/soundwalks/datafiles/sounds/sound_file_59508441c1fb5.mp3</value>
            </Data>
            <Data name="title">
                <value>Queens of Nambour</value>
            </Data>
            <Data name="desc">
                <value>A lesson in deportment, by June Upton</value>
            </Data>
            <Data name="images">
                <value>hotspot_image_5959afb6be89c, hotspot_images_5959afb6daaed, hotspot_images_5959afb706de5, hotspot_images_595aea22794fe, hotspot_images_5964336951ee9</value>
            </Data>
        </ExtendedData>
    </Placemark>

  </Document>
</kml>
# `.stx` File Format Definition

An `.stx` file is a subset of a [`.kml` file](https://developers.google.com/kml/documentation/kml_tut#basic_kml), with some additional extensions.

A `.stx` file can be loaded into software that can view a `.kml` without error (although not all the sound trail functionality will work).

### `<Document>`

All properties **MUST** be inside of a `<Document>` tag.

(We are looking at having a custom XLS)


### Supported propeties

Only a subset of the available KML properties are supported by a `.stx` file.


Soundfields can be represented in a `.stx` file as either **polygons** or **circles**.


##### Polygons

```xml
<Placemark id="1">
    <Polygon>
        <outerBoundaryIs>
            <LinearRing>
            <coordinates>
                152.84486,-26.758348 152.84604,-26.758348 152.84604,-26.757237 152.84486,-26.757237 152.84486,-26.758348
            </coordinates>
            </LinearRing>
        </outerBoundaryIs>
    </Polygon>
</Placemark>
```

##### Circles

A circle is a point, with an ExtendedData Data attribute to represent the radius.

```xml
<Placemark id="2">
    <Point>
        <coordinates>152.849538,-26.758128</coordinates>
    </Point>
    <ExtendedData>
        <Data name="radius">
            <value>104</radius>
        </Data>
    </ExtendedData>
</Placemark>
```



### Additional extension

The `ExtendedData` tag is used to include the soundtrail specific information, which allows cross compatability with standard KML files.

Using the namespace `http://soundtrails.com.au/soundtrailformat/1.0.0/soundtrail.xsd` allows soundtrail specific XML tags.

#### Inside of `<Document>`

The available tags are :
* **title**
* **desc**
* **imageBaseUrl**
* **audioBaseUrl**
* **images**
* **mapTilesUrl**  (This is a URL to a `.mbtiles` file to be used for displaying the map) OR
* **mapImageOverlayUrl**  (An option image to use for overlaying)
* **mapImageBounds** (A geo pair required if mapImageOverlayUrl is set)
* **author** (warning if not present)
* **copyright** (warning if not present)

For example : 

```xml
<ExtendedData>
    <Data name="title">
        <value>Nambour Sound Walk</value>
    </Data>
    <Data name="desc">
        <value>Drawing together history and heritage, local stories and great original music, the Nambour Heritage Soundtrail is a contemporary take on a truly historical town.</value>
    </Data>
    <Data name="imageBaseUrl">
        <value>https://soundtrails.com.au/soundwalks/datafiles/images/</value>
    </Data>
    <Data name="images">
        <value>soundwalk_image_596457907caef, soundwalk_image_596457b1a0841, soundwalk_image_596457d396bce,
        soundwalk_image_59645808f1741, soundwalk_image_596458482974e, hotspot_image_59768cde8e57d</value>
    </Data>
</ExtendedData>
```

#### Inside of `<Placemark>`

* **radius** 
  *(for a circle)*
* **title**
* **desc**
* **transcript** 
  *(text of the audio file, for hearing impared)*
* **audioFileUrl**  *(full URL path, or name relative to `audioBaseUrl`)*
* **audioLayer**
*(higher layer audio will play over the top of lower)* 
(def: 10)
* **areaDisplayStyle**
*(if this area is visible on a map, & how displayed ('area', 'none', 'pin', 'alert'))* 
(def: 'area')
* **audioPlayerVisibe** 
*(BOOL - show the audio player show for this audio)*
(def: true)
* **audioLoop**
*('never', 'on-completed', 'on-re-entry')*
(def: 'never')
* **audioPlayUntilEnd**
*(BOOL - should audio play until the end even if leaving triggger zone)*
(def: fase)
* **audioContinueOnReEntry**
*(BOOL - should the audio continue where it left off when re-entering the area)*
(def: true)
* **images** 
    *(a comma separated list of image file names or full URLS)*

For example : 

```xml
<Placemark id="336">
    <Point>
        <coordinates>152.9598328471183800,-26.6269910082151250</coordinates>
    </Point>
    <ExtendedData>
        <Data name="radius">
            <value>150</value>
        </Data>
        <Data name="audioFileUrl">
            <value>https://soundtrails.com.au/soundwalks/datafiles/sounds/sound_file_595082f26d231.mp3</value>
        </Data>
        <Data name="title">
            <value>Howard St and the Cane Train Haul</value>
        </Data>
        <Data name="desc">
            <value>Memories of the trains that went past and the chaos that sometime ensued.</value>
        </Data>
        <Data name="images">
            <value>hotspot_images_5959a829a004b, hotspot_images_5964336951ee9</value>
        </Data>
    </ExtendedData>
</Placemark>
```

## Full Sample 

**[Sample.stx](./Sample.soundtrail)**

## XSD

XSD (XML Schema Definition) is a World Wide Web Consortium (W3C) recommendation that specifies how to formally describe the elements in an Extensible Markup Language (XML) document. This description can be used to verify that each item of content in a document adheres to the description of the element in which the content is to be placed.

I generated an XSD for `.soundtrail` files using https://www.freeformatter.com/xsd-generator.html and with the Sample.soundtrail (and a poly added) as an input.

Here is the XSD for `.stx` files : **[soundtrail.v1.xsd](./soundtrail.v1.xsd)**
<html>
  <head>
  <title>My Now Amazing Webpage</title>
  <link rel="stylesheet" type="text/css" href="//cdn.jsdelivr.net/npm/slick-carousel@1.8.1/slick/slick.css"/>
  </head>
  <body>

  <div class="your-class">
    <div>your content 1</div>
    <div>your content 2</div>
    <div>your content 3</div>
  </div>

  <script type="text/javascript" src="http://code.jquery.com/jquery-3.4.1.min.js"></script>
  <script type="text/javascript" src="//cdn.jsdelivr.net/npm/slick-carousel@1.8.1/slick/slick.min.js"></script>

  <script type="text/javascript">
    $(document).ready(function(){
      $('.your-class').slick();
    });
  </script>

  </body>
</html>
