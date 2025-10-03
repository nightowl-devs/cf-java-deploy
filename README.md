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
        uses: nightowl-devs/cf-java-deploy@latest
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
| `output-dir` | No | `./cf-deploy-output` | Output directory |
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
          
      - name: Deploy Java Artifacts to Cloudflare Pages
        uses: nightowl-devs/cf-java-deploy@v1
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

## License

MIT see [LICENSE](LICENSE) file for details.