# Install Neo4j

This GitHub action installs a fresh Neo4j server and a suitable Java
runtime, sets the neo4j user password, and starts the server.
The server is installed in a subdirectory of the working directory.

This action uses the Neo4j community edition, available from
<https://neo4j.com/deployment-center/>.

Supported Neo4j versions are 2, 3, 4, 5 and 2025.

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
      NEO4J: "5.26.14"
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
   download URL, typically a string that looks like `2025.09.0`.
   An optional leading `v` will be ignored.
* `password` – The password to set for the default `neo4j` user. Some Neo4j
   versions need a string with at least eight characters. Required.
* `neo4j-dist-url` – Base URL for downloading Neo4j distributions.
   Default: `https://dist.neo4j.org`

The `neo4j-version` may be abbreviated. For example, given `5` or `5.26`,
the action will *try* to resolve this to the latest known subversion of
Neo4j 5.26.
This feature is currently experimental.

## Directories

Neo4j is installed in a directory named `neo4j-community-$VERSION`,
where VERSION is exactly the value of the `neo4j-version` output.

Java is installed via the [setup-java](https://github.com/actions/setup-java)
action, which uses the JVM pre-installed on the GH runner if available.
The full path is written to a file named `neo4j-javahome`, for convenience.

## Limitations

Running a server with disabled authentication is not currently implemented.
