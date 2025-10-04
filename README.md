![cf-java-deploy](https://socialify.git.ci/nightowl-devs/cf-java-deploy/image?custom_description=Deploy+java+artifacts+into+CloudFlare+pages+effortlessly.&custom_language=Cloudflare&description=1&issues=1&language=1&name=1&owner=1&pulls=1&stargazers=1&theme=Dark)
# cf-java-deploy


Easily deploy Java artifacts and Javadoc documentation to Cloudflare Pages, enabling you to host your own repository without the need for a dedicated server. This action also supports updating the latest version of your Java artifact in your README file, which is particularly useful for maintaining up-to-date examples.

## Usage

```yaml
name: Deploy Java to CF Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
          
      - name: Deploy Java Artifacts to Cloudflare Pages
        uses: nightowl-devs/cf-java-deploy@main
        with:
          cf-api-token: ${{ secrets.CF_API_TOKEN }}
          cf-account-id: ${{ secrets.CF_ACCOUNT_ID }}
          cf-project-name: maven-repo
          javadoc-url-marker: '<---!JAVADOCURL--->'
```

## Parameters

| Parameter | Required | Default | Description |
|----------|----------|----------|------|
| `java-version` | No | `17` | Java version |
| `distribution` | No | `temurin` | Java distribution |
| `build-system` | No | `auto` | Build system (`auto`, `gradle`, `maven`) |
| `version-conflict` | No | `bump` | Version conflict handling (`bump`, `replace`, `fail`) |
| `cf-api-token` | **Yes** | - | Cloudflare API token |
| `cf-account-id` | **Yes** | - | Cloudflare account ID |
| `cf-project-name` | **Yes** | - | Cloudflare Pages project name |
| `update-readme` | No | `true` | Update version in README files |
| `version-marker` | No | `<---!CURRENTVERSION--->` | Version marker to replace |
| `javadoc-url-marker` | No | `<---!JAVADOCURL--->` | Javadoc URL marker to replace |
| `include-javadoc` | No | `true` | Include Javadoc documentation |
| `output-dir` | No | `build/repo` | Output directory |
| `github-token` | No | `${{ github.token }}` | GitHub token for tags and releases |

## Example configuration

```yaml
name: Deploy Java to CF Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
          ref: main
          
      - name: Deploy Java Artifacts to Cloudflare Pages
        uses: nightowl-devs/cf-java-deploy@main
        with:
          java-version: '21'
          distribution: 'temurin'
          build-system: 'gradle'
          version-conflict: 'bump'
          cf-api-token: ${{ secrets.CF_API_TOKEN }}
          cf-account-id: ${{ secrets.CF_ACCOUNT_ID }}
          cf-project-name: maven-repo
          update-readme: 'true'
          version-marker: '<---!CURRENTVERSION--->'
          javadoc-url-marker: '<---!JAVADOCURL--->'
          include-javadoc: 'true'
          output-dir: './cf-output'
```

## Publishing Configuration

### Gradle

To configure publishing in a Gradle project, add the following to your `build.gradle` file:

```groovy
publishing {
    publications {
        mavenJava(MavenPublication) {
            from components.java
            artifactId = 'artifact-id' // Replace with your artifact ID
        }
    }
    repositories {
        maven {
            url = uri("${buildDir}/repo")
        }
    }
}
```

### Maven

For Maven projects, add the following to your `pom.xml` file:

```xml
<distributionManagement>
    <repository>
        <id>local-repo</id>
        <url>file://${project.build.directory}/repo</url>
    </repository>
</distributionManagement>
```

## License

MIT see [LICENSE](LICENSE) file for details.
