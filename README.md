# Install Neo4j

This GitHub action installs a fresh Neo4j server and a suitable Java
runtime, sets the neo4j user password, and starts the server. Server
and JRE are installed in subdirectories of the working directory.

This action uses the Neo4j community edition, available from
[https://neo4j.com/download-center/](https://neo4j.com/download-center/#community).

Supported Neo4j versions are 2, 3, 4, and 5.

[![Tests](https://github.com/johannessen/actions-install-neo4j/actions/workflows/tests.yaml/badge.svg)](https://github.com/johannessen/actions-install-neo4j/actions/workflows/tests.yaml)

## Synopsis

```yaml
# .github/workflows/tests.yaml

name: Testing Neo4j driver
on: push
jobs:
  tests:
    runs-on: ubuntu-latest
    env:
      NEO4J: "4.4.23"
    steps:

      - name: Install Neo4j
        uses: johannessen/actions-install-neo4j@v1
        with:
          neo4j-version: ${{ env.NEO4J }}
          password: xkcd_792

      - name: Prepare Test
        run: |
          export JAVA_HOME="$(cat neo4j-javahome)"
          neo4j-community-$NEO4J/bin/cypher-shell -u neo4j -p xkcd_792 < data.cypher
          ...
```

## Inputs

* `neo4j-version` – The full Neo4j version designation as it appears in the
   download URL, typically a string that looks like `5.9.0`.
* `password` – The password to set for the default `neo4j` user. Some Neo4j
   versions need a string with at least eight characters. Required.
* `neo4j-dist-url` – Base URL for downloading Neo4j distributions.
   Default: `https://dist.neo4j.org`

## Directories

Neo4j is installed in a directory named `neo4j-community-$VERSION`,
where VERSION is exactly the value of the `neo4j-version` input.

Java is installed in one of the following directories, depending
on the Neo4j version. The full path is written to a file named
`neo4j-javahome`, for convenience.

* `jdk8u392-b08-jre` for Neo4j 2/3
* `jdk-11.0.22+7-jre` for Neo4j 4
* `jdk-17.0.9+9-jre` for Neo4j 5

## Limitations

Running a server with disabled authentication is not currently implemented.
