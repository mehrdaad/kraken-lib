# kraken-lib configuration schema

## Overview

The kraken-lib configuration file is the interface between kraken and the user. Therefore it is important to maintain configuration file compatibly between kraken-lib releases.

Each version of kraken-lib supports one or more configuration file formats. The format is specified using [JSON Schema Draft 4](https://github.com/json-schema-org/json-schema-org.github.io/tree/master/draft-04) and is versioned. The schema version which each configuration file should conform to is specified by the value of the “version” key in the configuration file. For example, this configuration file:

```
---
version: v1
deployment:
  clusters:
    - name: krakenCluster
      network: 10.32.0.0/12
...
```

should conform to schema version v1.

Note that the configuration file itself is un-versioned outside of an external version control system.

[Understanding JSON Schema](https://spacetelescope.github.io/understanding-json-schema/) provides a good introduction to the JSON Schema standard.

## Schema changes

When changing the configuration file format be sure to update the schema file. By default JSON Schema will ignore
additional properties it finds in a document (i.e. these documents are still considered valid).
Setting ``` "additionalProperties": false ``` is recommended for all JSON schema definition files, to avoid invalid schemas.

kraken configuration files should be backwards compatible but may not be forward compatible. That is a configuration file generated with kraken-lib version x.y should be compatible with version kraken-lib version x+1. On the other hand, a configuration file generated with kraken-lib version x.y may not be compatible with kraken-lib version x-1.

Changes may be breaking or non-breaking. Changes which extend the schema by, for example, adding an __optional__ property, are non-breaking. Changes which make a __required__ property __optional__ are also non-breaking. All other syntactic, _and_ semantic, changes are generally breaking and should be avoided.

If it becomes necessary to make a breaking change, a new schema with a new version must be created. Previous schema versions should be supported for at least two kraken-lib releases or 3 months (whichever is longer). During this time, attempts to use configuration files generated by older kraken-lib releases will fail and a deprecation notice will be printed. In order to continue using these configuration files the --force flag must be passed to kraken.

Once a schema version is no longer supported the corresponding schema should be removed from kraken-lib in order to ensure configuration files using it fail to validate.
