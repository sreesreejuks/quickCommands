# Maven Quick Commands

## CI Build Debugging

Bump a dependency/artifact version across a POM (what CI runs before
`clean install`, to stamp the build with a version):
```bash
mvn -B -ntp -q versions:set -DnewVersion=<version> -f pom.xml
```
