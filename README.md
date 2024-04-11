# openapi-generator-bug2

Minimal reproducible example for [[BUG] On openapi-generator-maven-plugin v7.4.0 does not set @Valid annotation to array elements with common base](https://github.com/OpenAPITools/openapi-generator/issues/18358).

## Development

This repo is aimed to be opened in **[VSCode](https://code.visualstudio.com/)** with the **[Remote Development](https://code.visualstudio.com/docs/remote/remote-overview)** extension pack installed.

The development is done inside a **[Docker container](https://docker.com/)** that installs and configures the required tools on first run (the first time the Docker image is build might take some time).

Please, make sure you have **VSCode** with the **Remote Development extension pack** and **Docker** installed and working on your system before cloning the respository then clone de repository:

`git clone https://github.com/rubensa/openapi-generator-bug2.git`

For detailed instructions see: **[Developing inside a Container](https://code.visualstudio.com/docs/remote/containers)**.

## Project Organization

    ├── .devcontainer     <- VSCode Remote Development configuration
    │
    ├── src               <- Application source code
    │
    ├── .gitgnore         <- Git ignored files
    ├── pom.xml           <- Maven dependencies
    └── README.md         <- This file

## Showing the bug

You can see the bug by running the following command in a terminal inside the VSCode container:
```shell script
mvn clean compile
```

This will generate under **target/generated-sources/openapi/src/main/java/org/eu/rubensa/openapi/api/** folder the Java classes based on **src/main/resources/META-INF/resources/open-api/sample.yml** OpenAPI definition file before trying to compile the project.

The generated Java code will be something like (does not include `@Valid` in `List<Sample> sample`):

```java
    ...
    default ResponseEntity<Long> setSample(
        @Parameter(name = "Sample", description = "", required = true) @Valid@Size(min = 1)  @RequestBody List<Sample> sample
    ) {
        getRequest().ifPresent(request -> {
            for (MediaType mediaType: MediaType.parseMediaTypes(request.getHeader("Accept"))) {
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
            }
        });
        return new ResponseEntity<>(HttpStatus.NOT_IMPLEMENTED);

    }
    ...
```

If you modify the `pom.xml` file and change the `openapi-generator-maven-plugin` version to `7.3.0` the generated Java code will be something like (does include `@Valid` in `List<@Valid Sample> sample`):

```java
    ...
    default ResponseEntity<Long> setSample(
        @Parameter(name = "Sample", description = "", required = true) @Valid@Size(min = 1)  @RequestBody List<@Valid Sample> sample
    ) {
        getRequest().ifPresent(request -> {
            for (MediaType mediaType: MediaType.parseMediaTypes(request.getHeader("Accept"))) {
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
                if (mediaType.isCompatibleWith(MediaType.valueOf("application/problem+json"))) {
                    String exampleString = "Custom MIME type example not yet supported: application/problem+json";
                    ApiUtil.setExampleResponse(request, "application/problem+json", exampleString);
                    break;
                }
            }
        });
        return new ResponseEntity<>(HttpStatus.NOT_IMPLEMENTED);

    }
    ...
```