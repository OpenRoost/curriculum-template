# OpenRoost Curriculum Template

Reusable repository template for authoring [Open edX](https://openedx.org/) courses as
version-controlled [Open Learning XML (OLX)](https://docs.openedx.org/en/latest/educators/olx/index.html)
sources.

The repository is intended to provide the canonical starting point for OpenRoost course repositories.
Course content is maintained in Git, reviewed through pull requests, validated in CI, packaged as an
Open edX-compatible OLX archive, and imported into Open edX.

## Principles

* **OLX is the canonical source format.**
* Course sources live in Git rather than being maintained primarily in the LMS.
* Prefer standard Open edX mechanisms over custom abstractions.
* Keep the repository structure simple, explicit, and easy to review through Git diffs.
* Validation and packaging are performed through GitHub Actions.
* Avoid project-specific development runtimes and local tooling requirements unless they become necessary.

The template intentionally does not introduce an intermediate course DSL, Markdown-to-OLX compiler,
custom XBlocks, deployment automation, or other infrastructure that Open edX course authoring does not
require.

## Using the template

Once the initial course scaffold is available, create a new course repository using GitHub's
**Use this template** action.

Each generated repository will contain the OLX course sources, authoring schemas, validation workflows,
and documentation needed to start developing a course.

## License

This work is licensed under the
[Creative Commons Attribution 4.0 International License](LICENSE).
