# Authoring schemas

This directory contains schemas for validating and authoring Open edX OLX course sources.

The schemas are intended to provide early feedback while course content is developed in Git.
They complement Open edX import validation; they do not replace it or define the complete OLX
format.

Compatibility baseline: **Open edX Verawood.1**.

## Schemas

| File | Validates |
| --- | --- |
| `olx.xsd` | Studio-compatible OLX XML course structure and common components |
| `assets.schema.json` | `policies/<run>/assets.json` |
| `grading_policy.schema.json` | `policies/<run>/grading_policy.json` |
| `policy.schema.json` | `policies/<run>/policy.json` |

## OLX schema

`olx.xsd` covers the repository's baseline authoring subset:

- course, chapter, sequential, and vertical structure;
- HTML and video components;
- problem components;
- single-select problems;
- multi-select problems;
- text input problems;
- numerical input problems;
- dropdown problems.

The schema intentionally remains extensible where Open edX permits additional metadata or XBlocks.

It is an authoring schema rather than a normative definition of every OLX element supported by
Open edX.

## JSON schemas

The JSON schemas describe the policy files emitted and consumed by Open edX.

They intentionally differ in strictness:

- `grading_policy.schema.json` describes a small, stable policy structure and is comparatively strict;
- `policy.schema.json` allows additional course metadata because Open edX exports platform and
  feature-specific settings;
- `assets.schema.json` allows additional asset metadata emitted by Open edX.

## Using the schemas

Course source files should remain normal Open edX OLX files.

Do not add schema-specific metadata such as `$schema`, `xsi:noNamespaceSchemaLocation`, or similar
authoring hints to course files solely for validation purposes.

Validation tooling should associate each source file with its schema externally.

The repository's CI workflows are the canonical validation interface and should use the schemas from
this directory directly.

## Scope

Passing these schemas means that the covered source structure is well formed and matches the
supported authoring subset.

It does not guarantee that:

- referenced OLX files exist;
- referenced static assets exist;
- course identifiers and filenames agree;
- every XBlock is available on the target Open edX installation;
- the course can be imported successfully into a particular Open edX deployment.

Those checks belong to repository-level validation and Open edX import tooling.
