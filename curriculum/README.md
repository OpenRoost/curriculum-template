# Curriculum source

This directory contains the canonical Open edX course sources for this repository.

Course content is authored directly as
[Open Learning XML (OLX)](https://docs.openedx.org/en/latest/educators/olx/index.html)
and related Open edX course files.

The contents of this directory, except repository-only documentation such as this file, are packaged
into the `.tar.gz` archive imported into Open edX Studio.

## Open edX OLX documentation

The Open edX documentation is the primary reference for course authoring.

Useful starting points:

- [OLX documentation](https://docs.openedx.org/en/latest/educators/navigation/olx.html)
- [What is Open Learning XML?](https://docs.openedx.org/en/latest/educators/olx/what-is-olx.html)
- [OLX course structure](https://docs.openedx.org/en/latest/educators/olx/directory-structure.html)
- [OLX courseware structure](https://docs.openedx.org/en/latest/educators/olx/organizing-course/course-xml-file.html)
- [OLX course building blocks](https://docs.openedx.org/en/latest/educators/olx/organizing-course/course-structure-overview.html)
- [HTML components](https://docs.openedx.org/en/latest/educators/olx/components/html-components.html)
- [Problem components](https://docs.openedx.org/en/latest/educators/olx/components/problem-components.html)
- [Video components](https://docs.openedx.org/en/latest/educators/olx/components/video-components.html)
- [Course policies](https://docs.openedx.org/en/latest/educators/olx/policies/course.html)

The repository also provides authoring schemas in [`../schemas`](../schemas/).

## Course structure

Open edX Studio uses a fixed hierarchy for the primary courseware structure:

```text
Course
└── Section
    └── Subsection
        └── Unit
            └── Component
```

The corresponding OLX elements are:

```text
course
└── chapter
    └── sequential
        └── vertical
            └── html / problem / video / XBlock
```

Sections, subsections, units, and many standard components are stored as separate files and referenced
through their `url_name`.

For example:

```xml
<chapter display_name="Example Section">
  <sequential url_name="example_subsection"/>
</chapter>
```

references:

```text
sequential/example_subsection.xml
```

For file-backed OLX components, keep the referenced filename, without the `.xml` extension, equal to
the corresponding `url_name`.

## Directory layout

`curriculum/` is the repository source root for the course. It is not an additional directory in the
Open edX import archive.

### `course.xml`

The OLX entry point.

It identifies the Open edX organization, course identifier, and course run.

For example:

```xml
<course
  org="OpenRoost"
  course="example"
  url_name="run"
/>
```

The `url_name` identifies the course run defined by:

```text
course/run.xml
```

Treat the values in `course.xml` as course identity rather than ordinary course content. Replace
template values deliberately when creating a new course repository.

### `course/`

Contains course run definitions.

A run file contains course-level metadata and references the sections that make up the course.

For example:

```xml
<course display_name="Example Course">
  <chapter url_name="example_section"/>
</course>
```

### `chapter/`

Contains course sections.

Each file normally defines one `chapter` and references one or more subsections:

```xml
<chapter display_name="Example Section">
  <sequential url_name="example_subsection"/>
</chapter>
```

### `sequential/`

Contains course subsections.

Each file normally defines one `sequential` and references one or more units:

```xml
<sequential display_name="Example Subsection">
  <vertical url_name="example_unit"/>
</sequential>
```

Subsection-level grading, scheduling, and assessment settings may also be defined here.

### `vertical/`

Contains course units.

A unit groups the components displayed together to a learner:

```xml
<vertical display_name="Example Unit">
  <html url_name="example_text"/>
  <problem url_name="example_problem"/>
</vertical>
```

Standard file-backed components should normally be referenced from the unit.

Some XBlocks are represented inline instead of being stored in a separate component directory.
Follow the Open edX documentation for the specific XBlock rather than introducing repository-specific
wrappers.

### `html/`

Contains text components.

Studio-compatible text components normally consist of two files:

```text
html/<name>.xml
html/<name>.html
```

The XML file contains component metadata:

```xml
<html
  display_name="Example Text"
  filename="example_text"
/>
```

The corresponding `.html` file contains learner-facing HTML.

Prefer valid, semantic HTML. Keep styling and presentation-specific markup to the minimum required by
the content.

### `problem/`

Contains Open edX problem components.

Each problem is normally stored in a separate OLX XML file:

```text
problem/<name>.xml
```

Prefer standard Open edX problem elements when they satisfy the exercise requirements.

The repository authoring schema explicitly covers the five common simple problem types:

- single select;
- multi-select;
- text input;
- numerical input;
- dropdown.

Advanced CAPA problem types remain Open edX functionality, but are outside the explicitly modelled
schema subset.

### `video/`

Contains Open edX video component definitions.

Create this directory only when the course contains file-backed video components.

A video definition is referenced from a unit by its `url_name` in the same way as HTML and problem
components.

### `static/`

Contains static course assets such as images, PDFs, downloads, and other files referenced by course
content.

Keep binary assets here rather than embedding them into XML or HTML.

Do not commit generated files, temporary files, or build artifacts into this directory.

Create `static/` only when the course actually contains static assets.

### `policies/`

Contains Open edX course policy data.

A typical layout is:

```text
policies/
├── assets.json
└── <run>/
    ├── grading_policy.json
    └── policy.json
```

The `<run>` directory corresponds to the run selected by `course.xml`.

Authoring schemas for these files are provided in [`../schemas`](../schemas/).

### Other Open edX directories

Open edX supports additional course directories such as `about/`, `info/`, and `tabs/`.

Do not create them pre-emptively. Add them when the course actually requires the corresponding Open
edX feature and follow the current OLX documentation for their contents.

## Naming conventions

Use descriptive and stable source identifiers.

Prefer:

```text
chapter/getting_started.xml
sequential/install_python.xml
vertical/python_environment.xml
html/python_requirements.xml
problem/check_python_version.xml
```

over opaque generated names when authoring new content directly in Git.

Use lowercase ASCII identifiers with underscores:

```text
getting_started
install_python
first_exercise
```

For file-backed elements, the filename stem should match the `url_name` used to reference it:

```xml
<html url_name="python_requirements"/>
```

```text
html/python_requirements.xml
```

Learner-facing text belongs in `display_name`, not in filenames:

```xml
<chapter display_name="Getting Started">
```

```text
chapter/getting_started.xml
```

This allows display names to change without renaming files and updating references.

Treat identifiers as stable once course content has been published. Renaming a file-backed OLX
component also requires updating every reference to that identifier.

Studio exports often contain UUID-like identifiers. These are valid OLX identifiers. Do not rename
existing identifiers solely for cosmetic reasons when importing or migrating an existing course.

## Authoring practices

### Keep structure and content separate

Container files should primarily describe course structure.

Prefer:

```xml
<vertical display_name="Example Unit">
  <html url_name="concept_overview"/>
  <problem url_name="concept_check"/>
</vertical>
```

over placing large amounts of component content directly into structural files when Open edX provides
a standard file-backed representation.

This keeps changes small and improves Git diffs, reviews, and merge conflict resolution.

### Prefer Studio-compatible OLX

Open edX supports OLX structures that Studio does not necessarily export or reliably re-import.

For normal course development, prefer the current Studio-compatible structure documented by Open edX.

Do not introduce alternative layouts merely because the underlying XBlock model technically permits
them.

### Prefer one logical component per file

A source file should normally represent one structural element or one file-backed course component.

This improves:

- Git diffs;
- pull request review;
- merge conflict resolution;
- reference validation;
- navigation through the repository.

### Keep references explicit

Do not introduce custom indirection or repository-specific resolution rules.

A reference such as:

```xml
<problem url_name="concept_check"/>
```

should directly correspond to:

```text
problem/concept_check.xml
```

The same convention applies to sections, subsections, units, HTML components, videos, and other
file-backed OLX elements.

### Keep XML reviewable

Follow the repository formatting conventions from the root `.editorconfig`.

In particular:

- use two-space indentation;
- keep formatting predictable;
- split long elements across lines where that improves readability;
- avoid unrelated formatting changes in content pull requests;
- preserve a final newline.

Git diffs are part of the course authoring workflow and should remain easy to review.

### Do not add authoring-only metadata to course files

Do not add `$schema`, `xsi:noNamespaceSchemaLocation`, editor configuration, or other
repository-specific metadata to OLX and policy files solely to enable validation.

Course files should remain normal Open edX source files.

The repository validation workflow associates files with the schemas externally.

## Validation

Authoring schemas are maintained in [`../schemas`](../schemas/).

They provide structural validation and editor/tooling assistance, but schema validation alone cannot
verify the complete course.

Repository-level validation should additionally check:

- references between OLX files;
- referenced static assets;
- consistency between `url_name` values and filenames;
- policy files;
- construction of an Open edX import archive.

A successful Open edX import remains the final compatibility check.

## Packaging

`curriculum/` is a repository-level source directory.

Its contents form the root of the generated Open edX archive:

```text
course.xml
course/
chapter/
sequential/
vertical/
html/
problem/
policies/
static/
...
```

The directory itself must not become an additional archive level:

```text
# Wrong
curriculum/course.xml

# Correct
course.xml
```

Repository-only documentation such as this `README.md` must not be included in the generated course
archive.

Generated archives and other build artifacts must not be committed back into the curriculum source
tree.
