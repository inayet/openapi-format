# openapi-format

Format an OpenAPI document by ordering and filtering fields.

The openapi-format CLI can load an OpenAPI file, sorts the OpenAPI fields by ordering them in a hierarchical order, and
can output the file with clean indenting, to either JSON or YAML.

Next to the ordering, the CLI provides additional options to filter fields & parts of the OpenAPI document based on
flags, tags, methods and operationID's.

## Table of content
* [Use-cases](#use-cases)
* [Features](#features)
* [Installation](#installation)
    + [Local Installation (recommended)](#local-installation--recommended-)
    + [Global Installation](#global-installation)
    + [NPX usage](#npx-usage)
* [Command Line Interface](#command-line-interface)
* [OpenAPI format options](#openapi-format-options)
* [OpenAPI sort configuration options](#openapi-sort-configuration-options)
* [OpenAPI filter options](#openapi-filter-options)
* [CLI sort usage](#cli-sort-usage)
* [CLI filter usage](#cli-filter-usage)
* [CLI rename usage](#cli-rename-usage)
* [CLI configuration usage](#cli-configuration-usage)
* [Credits](#credits)

## Use-cases

When working on large OpenAPI documents or with multiple team members, the file can be become messy and difficult to
compare changes. By sorting it from time to time, the fields are all ordered in a structured manner, which will help you
to maintain the file with greater ease.

The filtering is a handy add-on to remove specific elements from the OpenAPI like internal endpoints, beta tags, ...
This can be useful in CI/CD pipelines, where the OpenAPI is used as source for other documents like Web documentation,
Postman collections, test suites, ...

## Features

- [x] Order OpenAPI fields in a default order
- [x] Order OpenAPI fields in a custom order
- [x] Filter OpenAPI files based on methods
- [x] Filter OpenAPI files based on flags
- [x] Filter OpenAPI files based on tags
- [x] Filter OpenAPI files based on operationID's
- [x] Rename the OpenAPI title
- [x] Support OpenAPI documents in JSON format
- [x] Support OpenAPI documents in YAML format
- [x] Format via CLI
- [x] Format via config files
- [x] Use as a Module
- [x] Support for OpenAPI 3.0
- [ ] Support for OpenAPI 3.1

## Installation

### Local Installation (recommended)

While possible to install globally, we recommend that you add the openapi-format CLI to the `node_modules` by using:

```shell
$ npm install --save openapi-format
```

or using yarn...

```shell
$ yarn add openapi-format
```

Note that this will require you to run the openapi-format CLI with `npx openapi-format your-openapi-file.yaml` or, if
you are using an older versions of npm, `./node_modules/.bin/openapi-format your-openapi-file.yaml`.

### Global Installation

```shell
$ npm install -g openapi-format
```

### NPX usage

To execute the CLI without installing it via npm, use the npx method

```shell
$ npx openapi-format your-openapi-file.yaml
```

## Command Line Interface

```
openapi-format.js <input-file> -o [ouptut-file] [options]

Arguments:
  infile   the OpenAPI document, can be either a .json or .yaml file
  outfile  the output file is optional and be either a .json or .yaml file. Files that end in `.json` will be formatted 
  as JSON files that end in `.yaml` or `.yml` will be YAML format
  

Options:

  -o, --output     Save the formated OpenAPI file as JSON/YAML            [path]
  
  --sortFile       The file to specify custom OpenAPI fields ordering     [path]
  --filterFile     The file to specify filter setting                     [path]
    
  --no-sort        Don't sort the file                                 [boolean]
  --rename         Rename the OpenAPI title                             [string]

  --configFile     The file with all the format config options            [path]
  
  --json           Prints the file to stdout as JSON                   [boolean]
  --yaml           Prints the file to stdout as YAML                   [boolean]
  
  --help           Show help                                           [boolean]
  --verbose        Output more details of the filter process             [count]
```

## OpenAPI format options

| Parameter    | Alias         | Description                                                                 | Input type   | Default          | Required/Optional |
|--------------|---------------|-----------------------------------------------------------------------------|--------------|------------------|-------------------|
| file         |               | the original OpenAPI file                                                   | path to file |                  | required          |
| --output     | -o            | save the formatted OpenAPI file as JSON/YAML                                | path to file |                  | optional          |
| --sortFile   | -s            | the file to specify custom OpenAPI fields ordering                          | path to file | defaultSort.json | optional          |
| --filterFile | -f            | the file to specify filter setting                                          | path to file |                  | optional          |
| --no-sort    |               | don't sort the file                                                         | boolean      | FALSE            | optional          |
| --rename     |               | rename the OpenAPI title                                                    | string       |                  | optional          |
| --configFile | -c            | the file with all the format config options                                 | path to file |                  | optional          |
| --json       |               | prints the file to stdout as JSON                                           |              | FALSE            | optional          |
| --yaml       |               | prints the file to stdout as YAML                                           |              | FALSE            | optional          |
| --verbose    | -v, -vv, -vvv | verbosity that can be increased, which will show more output of the process |              |                  | optional          |
| --help       | h             | display help for command                                                    |              |                  | optional          |

## OpenAPI sort configuration options

The CLI will sort the OpenAPI document in the defined order liked defined per OpenAPI key/element. The fields that are
not specified will keep their order like it is in the original OpenAPI document, so only defined fields will be
re-ordered.

The default sorting based on the defined order (listed in the table below), which is stored in
the [defaultSort.json](https://github.com/thim81/openapi-format/blob/main/defaultSort.json) file. 

You can easily modify this by specifying your own ordering per key, which can passed on to the CLI (see below for an
example on how to do this).

| Key         | Ordered by                                                                                                      | OpenAPI reference                                                         |
| ----------- | ----------------------------------------------------------------------------------------------------------------| ------------------------------------------------------------------------- |
| root        | - openapi<br>\- info<br>\- servers<br>\- paths<br>\- components<br>\- tags<br>\- x-tagGroups<br>\- externalDocs | https://spec.openapis.org/oas/v3.0.3.html#openapi-object                  |
| get         | - operationId<br>\- summary<br>\- description<br>\- parameters<br>\- requestBody<br>\- responses                | https://spec.openapis.org/oas/v3.0.3.html#operationObject                 |
| post        | - operationId<br>\- summary<br>\- description<br>\- parameters<br>\- requestBody<br>\- responses                | https://spec.openapis.org/oas/v3.0.3.html#operationObject                 |
| put         | - operationId<br>\- summary<br>\- description<br>\- parameters<br>\- requestBody<br>\- responses                | https://spec.openapis.org/oas/v3.0.3.html#operationObject                 |
| patch       | - operationId<br>\- summary<br>\- description<br>\- parameters<br>\- requestBody<br>\- responses                | https://spec.openapis.org/oas/v3.0.3.html#operationObject                 |
| delete      | - operationId<br>\- summary<br>\- description<br>\- parameters<br>\- requestBody<br>\- responses                | https://spec.openapis.org/oas/v3.0.3.html#operationObject                 |
| parameters  | - name<br>\- in<br>\- description<br>\- required<br>\- schema                                                   | https://spec.openapis.org/oas/v3.0.3.html#parameterObject                 |
| requestBody | - description<br>\- headers<br>\- content<br>\- links                                                           | https://spec.openapis.org/oas/v3.0.3.html#request-body-object             |
| responses   | - description<br>\- headers<br>\- content<br>\- links                                                           | https://spec.openapis.org/oas/v3.0.3.html#responses-object                |
| content     | (By alphabet)                                                                                                   | https://spec.openapis.org/oas/v3.0.3.html#responses-object                |
| components  | - parameters<br>\- schemas                                                                                      | https://spec.openapis.org/oas/v3.0.3.html#components-object               |
| schema      | - description<br>\- type<br>\- items<br>\- properties<br>\- format<br>\- example<br>\- default                  | https://spec.openapis.org/oas/v3.0.3.html#schemaObject                    |
| schemas     | - description<br>\- type<br>\- items<br>\- properties<br>\- format<br>\- example<br>\- default                  |                                                                           |
| properties  | - description<br>\- type<br>\- items<br>\- format<br>\- example<br>\- default<br>\- enum                        |                                                                           |

## OpenAPI filter options

By specifying the desired filter values for the available filter types, the openapi-format CLI will strip out any
matching item from the OpenAPI document. You can combine multiple types to filter out a range of OpenAPI items.

For more complex use-cases, we can advise the excellent https://github.com/Mermade/openapi-filter package, which has
really extended options for filtering OpenAPI documents.

| Type         | Description                   | Type  | Examples                         |
|--------------|-------------------------------|-------|----------------------------------|
| methods      | a list OpenAPI methods.       | array | ['get','post','put']             |
| tags         | a list OpenAPI tags.          | array | ['pet','user']                   |
| operationIds | a list OpenAPI operation ID's.| array | ['findPetsByStatus','updatePet'] |
| flags        | a list of custom flags        | array | ['x-exclude','x-internal']       |

Some more details on the available filter types:

- **methods**: Refers to the "Path Item Object" http://spec.openapis.org/oas/v3.0.3.html#operationObject

This will remove all fields and attached fields that match the verbs. In the example below, this would mean that
all `get`, `put`, `post` items would be removed from the OpenAPI document.

```yaml
openapi: 3.0.0
info:
    title: API
    version: 1.0.0
paths:
    /pets:
        get:
            summary: Finds Pets by status
        put:
            summary: Update an existing pet
```  

- **tags**: Refers to the "tags" field from the "Operation
  Object" https://spec.openapis.org/oas/v3.0.3.html#operationObject

This will remove all fields and attached fields that match the tags. In the example below, this would mean that all
items with the tags `pet` or `user` would be removed from the OpenAPI document.

For example:

```yaml
openapi: 3.0.0
info:
    title: API
    version: 1.0.0
paths:
    /pets:
        put:
            tags:
                - pet
            summary: Update an existing pet
```  

- **operationIds**: Refers to the "operationId" field from the "Operation
  Object" https://spec.openapis.org/oas/v3.0.3.html#operationObject

This will remove specific fields and attached fields that match the operation ID's. In the example below, this would
mean that the item with operationID `findPetsByStatus` would be removed from the OpenAPI document.

For example:

```yaml
openapi: 3.0.0
info:
    title: API
    version: 1.0.0
paths:
    /pets:
        get:
            operationId: findPetsByStatus
```

- **flags**: Refers to a custom property which can be set on any field in the OpenAPI document.

This will remove all fields and attached fields that match the flags. In the example below, this would mean that all
items with the flag `x-exclude` would be removed from the OpenAPI document.

For example:

```yaml
openapi: 3.0.0
info:
    title: API
    version: 1.0.0
paths:
    /pets:
        get:
            x-exclude: true
```

## CLI sort usage

- Format a spec with the default sorting and saves it as a new JSON file

```shell
$ openapi-format openapi.json -o openapi-formatted.json
```

-Format a OpenAPI JSON document with the default sorting and saves it as a new YAML file

```shell
$ openapi-format openapi.json -o openapi.yaml
```

- Format a OpenAPI document with the default sorting and output it as JSON to STDOUT

```shell
$ openapi-format openapi.json --json
```

- Format a OpenAPI document with the default sorting and output it as YAML to STDOUT

```shell
$ openapi-format openapi.json --yaml
```

- Format a OpenAPI JSON document with the default sorting and save it as YAML

```shell
$ openapi-format openapi.json -o openapi.yaml
```

- Format a OpenAPI document but skip the sorting and save it as a new JSON file

example:

```shell
$ openapi-format openapi.json -o openapi-formatted.json --no-sort
```

Which should keep the OpenAPI fields in the same order. This can be needed, when you only want to do a filtering or
rename action.


## CLI filter usage

- Format a OpenAPI document by filtering fields, default sorting and saves it as a new file

When you want to strip certain flags, tags, methods, operationID's, you can pass a `filterFile` which contains the
specific values for the flags, tags, methods, operationID's.

This can be useful to combine with the sorting, to end-up with an order and filtered OpenAPI document.

example:

```shell
$ openapi-format openapi.json -o openapi-formatted.json --filterFile customFilter.yaml
```

where the `customFilter.yaml` would contain a combination of all the elements you want to filter out.

```yaml
flags:
    - x-visibility
flagValues: [ ]
tags: [ ]
operationIds:
    - addPet
    - findPetsByStatus
```

## CLI rename usage

- Format a OpenAPI document by changing the title and saves it as a new JSON file

During CI/CD pipelines, you might want to create different results of the OpenAPI document. Having the option to rename
them, might make it easier to work with the results, so that is why we provide this command option.

```shell
$ openapi-format openapi.json -o openapi.json --rename "OpenAPI Petstore - OpenAPI 3.0"
```

which results in

- before

```json
{
    "openapi": "3.0.2",
    "info": {
        "title": "Petstore - OpenAPI 3.0",
```

- after

```json
{
    "openapi": "3.0.2",
    "info": {
        "title": "OpenAPI Petstore - OpenAPI 3.0",
```

## CLI configuration usage

All the CLI options can be managed in separate configuration file and passed along the openapi-format command. This will
make configuration easier, especially in CI/CD implementations where the configuration can be stored in version control
systems.

example:

```shell
$ openapi-format openapi.json --configFil openapi-format-options.json
```

The formatting will happen based on all the options set in the `openapi-format-options.json` file. All the
available [OpenAPI format options](https://github.com/thim81/openapi-format#openapi-format-options) can be used in the
config file.

## Credits

This package is inspired by
the [@microsoft.azure/format-spec](https://www.npmjs.com/package/@microsoft.azure/format-spec) from @fearthecowboy. The
original code was not available on Github, with the last update was 3 years ago, so to improve support and extend it we
tried to reproduce the original functionality.

The filter capabilities from `openapi-format` are a light version inspired by the work from @MikeRalphson on
the https://github.com/Mermade/openapi-filter package.
