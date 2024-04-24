## Netzgrafik-Editor data export/import (JSON) 
The complete data export can be used for data exchange between different network graphics/variants, merging network graphics, or for user-specific customization, 
including metadata such as colors and train categories, among others.

By exporting or importing data in JSON format, information can be efficiently and flexibly exchanged or connected between 
different systems or applications. JSON format is a widely used format for exchanging structured data and allows for easy 
integration and processing of information in third-party applications. Additionally, it provides a readable and easily 
understandable representation of data, which facilitates manual editing.

[DATA MODEL](DATA_MODEL.md)

## JSON Description (basic data structure)

```json
{
  "nodes": [],
  "trainrunsections": [],
  "trainruns": [],
  "resources": [],
  "metadata": {},
  "freeFloatingTexts": [],
  "labels": [],
  "labelGroups": [],
  "filterData": {}
}
```

- **nodes**: Represents the nodes in the Netzgrafik.
- **trainrunsections**: Contains the sections of trainruns.
- **trainruns**: Represents the trainruns in the Netzgrafik.
- **resources**: Contains an abstraction for resources related to the Netzgrafik (between nodes - edges) *not yet implemented*.
- **metadata**: Stores metadata information such as colors, train categories, etc.
- **freeFloatingTexts**: Contains any freely placed texts in the Netzgrafik, notes.
- **labels**: Represents labels associated with nodes or trainruns.
- **labelGroups**: Groups labels together for easier management and to classify the labels into trainrun labels, node labels and note labes,.
- **filterData**: Contains data related to filtering the Netzgrafik.

This is the basic data structure to represent the various elements in a JSON description. You can populate each element with the specific data relevant to your scenario.

See also [DATA_MODEL.md : business orientated description](https://github.com/SchweizerischeBundesbahnen/netzgrafik-editor-frontend/blob/documentation/JSON_DATA/documentation/DATA_MODEL.md#business-orientated-description)

---

### META Data

```json
"metadata": {
  "netzgrafikColors": [],
  "trainrunCategories": [],
  "trainrunFrequencies": [],
  "trainrunTimeCategories": []
}
```

- **netzgrafikColors**: Represents the user defined colors which can be used in the Netzgrafik.
- **trainrunCategories**: Contains the categories of train runs.
- **trainrunFrequencies**: Represents the frequencies at which trainruns operates.
- **trainrunTimeCategories**: Contains the time categories for trainruns, such as all weekdays, only weekend and so on.

#### netzgrafikColors

```json
"netzgrafikColors": [
  {
    "id": 0,
    "colorRef": "GoldenColoring",
    "color": "#816756",
    "colorFocus": "#2F3A4C",
    "colorMuted": "#D5BE87",
    "colorRelated": " #ADA672",
    "colorDarkMode": "#ADA672",
    "colorDarkModeFocus": "#D5BE87",
    "colorDarkModeMuted": "#2F3A4C",
    "colorDarkModeRelated": "#816756"
  },
]
```

- **id**: Technical identifier (key), must be unique : numeric
- **colorRef**: Must be unique : string
- **color** :  HTML color (HEX) used when a trainrun is rendered : string
- **colorFocus** : HTML color (HEX) used when a trainrun is rendered as focused : string
- **colorMuted** : HTML color (HEX) used when a trainrun is rendered as muted : string
- **colorRelated** : HTML color (HEX) used when a trainrun is rendered as related to another : string 
- **colorDarkMode** : HTML color (HEX) used when a trainrun is rendered (dark mode) : string
- **colorDarkModeFocus** : HTML color (HEX) used when a trainrun is rendered as focused (dark mode) : string
- **colorDarkModeMuted** : HTML color (HEX) used when a trainrun is rendered as muted (dark mode) : string
- **colorDarkModeRelated** : HTML color (HEX) used when a trainrun is rendered as related to another (dark mode) : string

The Netzgrafik Editor has some default implemented colors with unique predefined colorRefs. Each colorRef must be unique (key). 
Therefore, those predefined colorRefs cannot be used for a user-specific netzgrafikColors declaration. The predefined colors are:
```
defaults: ColorRefType[] = ["EC", "IC", "IR", "RE", "S", "G", "GEX"]
```
For more detail have a look into the 
[netzgrafikColoring.service.ts](https://github.com/SchweizerischeBundesbahnen/netzgrafik-editor-frontend/blob/main/src/app/services/data/netzgrafikColoring.service.ts#L473) and also [src/app/models/netzgrafikColor.model.ts](https://github.com/SchweizerischeBundesbahnen/netzgrafik-editor-frontend/blob/main/src/app/models/netzgrafikColor.model.ts)


#### trainrunCategories 

```JSON
   "trainrunCategories": [
      {
        "id": 0,
        "name": "International",
        "order": 0,
        "colorRef": "GoldenColoring",
        "shortName": "EC",
        "fachCategory": "HaltezeitIPV",
        "sectionHeadway": 2,
        "nodeHeadwayStop": 2,
        "nodeHeadwayNonStop": 2,
        "minimalTurnaroundTime": 4
      },
]
```

- **id**: Technical identifier (key), must be unique : numeric
- **name**: The full name of the trainrun categories : string 
- **order**: The full position (sorting) of the trainrun categories in the list and as well used for the rendering order (heuristic) of the trainruns sorting : numeric 
- **colorRef**: Reference to a colorRef : netzgrafikColors.colorRef : string
- **shortName**: The short name of the trainrun categories. This is rendered in the Netzgrafik as prefix to the trainrun name : string
- **fachCategory** : Reference to the fachCategory : string
- **sectionHeadway** : Temporal distance (headway) in minute, when a train is traveling through a section : numeric 
- **nodeHeadwayStop** : Temporal distance (headway) in minute, when a train is stopping at a node (station) : numeric 
- **nodeHeadwayNonStop** : Temporal distance (headway) in minute, when a train is not stopping at a node (station) : numeric 
  

#### trainrunCategories

```json
"trainrunFrequencies": [
  {
    "id": 4,
    "name": "verkehrt zweistündlich (gerade)",
    "order": 0,
    "offset": 0,
    "frequency": 120,
    "shortName": "120",
    "linePatternRef": "120"
  },
  {
    "id": 5,
    "name": "verkehrt zweistündlich (ungerade)",
    "order": 0,
    "offset": 60,
    "frequency": 120,
    "shortName": "120+",
    "linePatternRef": "120"
  }
]
```

- **id**: Technical identifier (key), must be unique : numeric
- **name**:  The full name of the trainrun frequency : string
- **order**: The rendering order in the list : numeric
- **offset**: Frequency offset in minute - when the frequency start after midnight, e.g. 2h frequency can start at midnight (00:00 / even) or at (01:00 / odd) : numeric
- **frequency**: Frequency in minute : numeric
- **shortName** : Short name : string
- **linePatternRef** : Reference to the line rendering pattern: 1, 2, 3, 4 lines and as well used for dotted, ... : string

The defined line pattern which can be used are 
```
LinePatternRefs {
  "120", // -.-.-.  ; unique indentifier
  "60", // ----- ; unique indentifier
  "30", // ==== ; unique indentifier
  "20", // three lines  ; unique indentifier
  "15", // four lines ; unique indentifier
}
```

#### trainrunTimeCategories

```json
"trainrunTimeCategories": [
  {
    "id": 0,
    "name": "verkehrt uneingeschränkt",
    "order": 0,
    "weekday": [
      1,
      2,
      3,
      4,
      5,
      6,
      7
    ],
    "shortName": "7/24",
    "linePatternRef": "7/24",
    "dayTimeInterval": []
  },
  {
    "id": 1,
    "name": "verkehrt zur Hauptverkehrszeit",
    "order": 1,
    "weekday": [
      1,
      2,
      3,
      4,
      5,
      6,
      7
    ],
    "shortName": "HVZ",
    "linePatternRef": "HVZ",
    "dayTimeInterval": [
      {
        "to": 420,
        "from": 360
      },
      {
        "to": 1140,
        "from": 960
      }
    ]
  }
]
```

- **id**: Technical identifier (key), must be unique : numeric
- **name**:  The full name of the trainrun time category : string
- **order**: The rendering order in the list : numeric
- **weekday**: Weekdays when the trainrun is operated / Monday = 1, Tuesday = 2, ... , Sunday = 7 : array numeric
- **shortName** : Short name : string
- **linePatternRef** : Line pattern how to render the trainrun time category
- **dayTimeInterval** : can be empty : dayTimeIntervalElement - JSON


The defined line pattern which can be used are 
```
LinePatternRefs {
  TimeCat7_24 = "7/24", // . . . ; unique indentifier
  TimeCatHVZ = "HVZ", // . : . : ; unique indentifier
  TimeZeitweise = "ZEITWEISE", // : - : - ; unique indentifier
}
```

Define the dayTimeIntervalElement
```json
{
  "to": 1140,
  "from": 960
}
```
- **from** : interval starts at time (include), in minute : numeric
- **to** : interval ends at time (include), in minute : numeric 