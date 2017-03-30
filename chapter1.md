# IMARD LMS API

In this document the definitions, entities and verbs working within IMARD LMS are described. The code descriptions for each item is presented in the format of JSON Schema. **IMPORTANT:** none of the provided code descriptions is to be considered a standalone document rather than a part of one big schema.

## Definitions

### UNID

UNID stands for _Unique Node Identifier_ — a unique string to find, fetch, associate with and identify by nodes.

### Locale

The _lingual locale_ that is used to recognize the language of learning materials.

## Entities

### Node

The `Node` class represents learning node — a single indivisible waypoint in IMARD learning process. Somethig is a `Node` if it is a minimal specific achievable skill or a descrete obtainable knowledge.

Each node can have the following qualities:

* `UNID` — [_Unique Node Itentifier_](#definitions-unid "definitions/UNID");
* `locale` — [_lingual locale_](#locale);
* `name` — brief description of the skill or knowledge that is obtainable upon examining the node;
* `description` — more thorough description of subjects and activities involved in node's materials;
* `resolutionStatus` — tells whether are there materials accessible via IMARD node behind the described name and description. Each node can be in one of two options:
  * `resolved` — there are actual learning materials behind given `name` and `description`. The resolved status guarantees that one can access a working learning module via `UNID` and it would answer to the given properties;
  * `unresolved` — there are no available materials to answer given `name` and `description` within IMARD system yet.

Nodes can relate to each other via the following verbs:

* `dependsOn` — under this verb would be a list of nodes, knowledge and skills described in which are required for mastering ones represented by this current node. Each dependable node is represented with its `UNID`. This verb enables hierarchical structuring of the learning materials;
* `localizes` — under thes verd would be a list of nodes, knowledge and skills described in which are same or very similar to ones represented by this current node, however written in different languages. This verb enables dynamic multilingual learning.

The main body of `Node` is a webpage that is accessible via `UNID`. The page may contain learning materials and media along with the glossary.

```JSON
{
    "title": "Node object schema",
    "type": "object",
    "properties": {
        "UNID": { "$ref": "definitions/UNID"},
        "locale": {
            "$ref": "definitions/locale"
        },
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
            "description": "Tells the system whether are there materials accessible via IMARD node behind the described name and description",
            "enum": [
                "resolved",
                "unresolved"
            ]
        },
        "dependsOn": {
            "type": "array",
            "items": {
                "$ref": "definitions/UNID"
            },
            "maxItems": 4
        },
        "localizes": {
            "type": "array",
            "items": {
                "$ref": "definitions/UNID"
            }
        }
    },
    "additionalItems": false,
    "required": [
        "name", "description", "resolutionStatus", "locale"
    ]
}
```



