# IMARD LMS API

In this document the definitions, entities and verbs working within IMARD LMS are described. The code descriptions for each item is presented in the format of JSON Schema. **IMPORTANT:** none of the provided code descriptions is to be considered a standalone document rather than a part of one big schema.

## Definitions

### UNID

UNID stands for _Unique Node Identifier_ — a unique string to find, fetch, associate with and identify by nodes. UNID is also a URI of its respective node. Definition in the schema:
```JSON
// ... in definitions
"UNID": {
    "type": "string",
    "title": "URI of a IMARD learnin node"
}
// ...
```

### Locale

The _lingual locale_ that is used to recognize the language of learning materials. Definition in the schema:
```JSON
// ... in definitions
"locale": {
    "type": "string",
    "title": "Locale",
    "description": "Describes to l10n of the learning node",
    "enum": [
        "RU-ru",
        "EN-en"
    ]
}
// ...
```

## Entities

### Node

The `Node` class represents learning node — a single indivisible waypoint in IMARD learning process. Somethig is a `Node` if it is a minimal specific achievable skill or a descrete obtainable knowledge.

Each node can have the following qualities:

* `id` — [_Unique Node Itentifier_](#definitions-unid "definitions/UNID");
* `locale` — [_lingual locale_](#locale);
* `label` — brief description of the skill or knowledge that is obtainable upon examining the node;
* `description` — more thorough description of subjects and activities involved in node's materials;
* `resolutionStatus` — tells whether are there materials accessible via IMARD node behind the described name and description. Each node can be in one of two options:
  * `resolved` — there are actual learning materials behind given `name` and `description`. The resolved status guarantees that one can access a working learning module via `UNID` and it would answer to the given properties;
  * `unresolved` — there are no available materials to answer given `name` and `description` within IMARD system yet.

Nodes can relate to each other via the following verbs:

* `dependsOn` — under this verb would be a list of nodes, knowledge and skills described in which are required for mastering ones represented by this current node. Each dependable node is represented with its `UNID`. This verb enables hierarchical structuring of the learning materials;
* `localizes` — under thes verd would be a list of nodes, knowledge and skills described in which are same or very similar to ones represented by this current node, however written in different languages. This verb enables dynamic multilingual learning.

The main body of `Node` is a webpage that is accessible via `UNID`. The page may contain learning materials and media along with the glossary.

```JSON
// ... in definitions
"node": {
    "title": "IMARD learning node",
    "description": "A single indivisible waypoint in IMARD learning process",
    "type": "object",
    "properties": {
        "id": { 
            "$ref": "#/definitions/UNID"
        },
        "locale": {
            "$ref": "#/definitions/locale"
        },
        "label": {
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
                "$ref": "#/definitions/UNID"
            },
            "maxItems": 4
        },
        "localizes": {
            "type": "array",
            "items": {
                "$ref": "#/definitions/UNID"
            }
        }
    },
    "additionalItems": false,
    "required": [
        "name", "description", "resolutionStatus", "locale"
    ]
}
// ...
```

### Hierarchy
`Hierarchy` is a tree of several interconnected instances of `Node` class described in [JSON Graph Forman (JGF)](http://jsongraphformat.info). Although a `Hierarchy` should be valid against the [JGF v3.0 Schema](http://jsongraphformat.info/child-schemas/bel-json-graph-3.0.schema.json) we provide an adjusted version of the same schema definitions here. Things to note in this:

 - `"metadata"` is always `null`. JGF reserves this property solely for biological data, it has no use here;
 - `"items"` array can contain simple objects linked to nodes via UNID as well as fully-described nodes;
 - Both `"dependsOn"` and `"localizes"` verbs are supported as `"relation"` options so we could support multilingual learning experience.

```JSON
// ... in definitions
"hierarchy": {
    "title": "IMARD nodes hierarchy",
    "description": "An ordered set of waypoints of learning process within IMARD",
    "type": "object",
    "additionalProperties": false,
    "properties": {
        "graph": {
            "$ref": "#/definitions/graph"
        }
    },
    "required": [
        "graph"
    ]
},
"graph": {
    "type": "object",
    "additionalProperties": false,
    "title": "Graph or Network data",
    "properties": {
        "label": {
            "type": "string",
            "title": "Graph Label",
            "description": "Graph label"
        },
        "directed": true,
        "type": "IMARD learning tree",
        "metadata": null,

        "nodes": {
            "type": [
                "array",
                "null"
            ],
            "items": {
                "oneOf": [
                    {
                        "type": "object",
                        "additionalProperties": false,
                        "properties": {
                            "id": {
                                "$ref": "#/definitions/UNID"
                            },
                            "label": {
                                "type": "string"
                            },
                            "metadata": null,
                        }
                    },
                    {
                        "$ref": "#/definitions/node"
                    }
                ]
            },
            "required": [
                "id"
            ]
        },

        "edges": {
            "type": [
                "array",
                "null"
            ],
            "items": {
                "type": "object",
                "additionalProperties": false,
                "properties": {
                    "source": {
                        "$ref": "#/definitions/UNID"
                    },
                    "target": {
                        "$ref": "#/definitions/UNID"
                    },
                    "relation": {
                        "type": "string",
                        "title": "Edge relationship",
                        "description": "Relationship between nodes in edge - may be directed or undirected",
                        "enum": [
                            "dependsOn",
                            "localizes"
                        ]
                    },
                    "directed": true,
                    "label": {
                        "type": "string"
                    }
                },
                "required": [
                    "source",
                    "target"
                ]
            }
        }
    }
}
// ...
```

