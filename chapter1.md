# IMARD LMS API

In this document the definitions, entities and verbs working within IMARD LMS are described. The code descriptions for each item is presented in the format of JSON Schema. **IMPORTANT:** none of the provided code descriptions is to be considered a standalone document rather than a part of one big schema.

## Definitions

### UNID

UNID stands for _Unique Node Identifier_ — a unique string to find, fetch, associate with and identify by nodes.

## Entities

### Node

The `Node` class represents learning node - a single indivisible waypoint in IMARD learning process. Somethig is a `Node` if it is a minimal specific achievable skill or a descrete obtainable knowledge.

```JSON
{
    "title": "Node object schema",
    "type": "object",
    "properties": {
        "UNID": { "$ref": "definitions/UNID"},
        "name": {
            "type": "string",
            "description": "Briefly describes skill or knowledge obtainable by a student upon examining the node"
        },
        "description": {
            "type": "string",
            "description": "More thorough description of subjects and activities involved in node's materials"
        },
        "resolutionStatus": {
            "type": "string",
            "enum": [
                "resolved",
                "unresolved"
            ]
        },
        "dependancies": {
            "type": "array",
            "items": {
                "$ref": "definitions/UNID"
            },
            "maxItems": 4
        }
    },
    "additionalItems": false,
    "required": [
        "name", "resolutionStatus"
    ]
}
```



