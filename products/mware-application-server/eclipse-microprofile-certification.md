# Eclipse MicroProfile Certification

## Eclipse MicroProfile Certification Summary

Payara Server is a certified [Eclipse Microprofile](https://microprofile.io/) implementation. As proof of compatibility, output of MicroProfile Compatibility Test Suite tests are presented for each certified version in each release.

## TCK Results

As required by the [Eclipse Foundation Technology Compatibility Kit License](https://www.eclipse.org/legal/tck.php), following is a summary of the TCK results for releases of MicroProfile 6.0.

**Payara Server 6.2023.10 Full Profile Certification Summary**

*   Product Name, Version and download URL (if applicable):

    | ![JakartaEE Logo compatible color](https://docs.payara.fish/community/docs/\_images/JakartaEE\_Logo\_compatible-color.png) | <p><br><br><a href="https://www.payara.fish/downloads/payara-platform-community-edition/">Payara Server 6.2023.10</a></p> |
    | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
*   Specification Names, Versions and download URLs:

    [MicroProfile Config 3.0.2](https://download.eclipse.org/microprofile/microprofile-config-3.0.2/microprofile-config-spec-3.0.2.html) [MicroProfile Fault Tolerance 4.0.2](https://download.eclipse.org/microprofile/microprofile-fault-tolerance-4.0.2/microprofile-fault-tolerance-spec-4.0.2.html) [MicroProfile JWT Auth 2.1](https://download.eclipse.org/microprofile/microprofile-jwt-auth-2.1/microprofile-jwt-auth-spec-2.1.html) [MicroProfile Metrics 5.0.0](https://download.eclipse.org/microprofile/microprofile-metrics-5.0.0/microprofile-metrics-spec-5.0.0.html) [MicroProfile OpenAPI 3.1](https://download.eclipse.org/microprofile/microprofile-open-api-3.1/microprofile-openapi-spec-3.1.html) [MicroProfile OpenTracing 3.0](https://download.eclipse.org/microprofile/microprofile-opentracing-3.0/microprofile-opentracing-spec-3.0.html) [MicroProfile Rest Client 3.0](https://download.eclipse.org/microprofile/microprofile-rest-client-3.0/microprofile-rest-client-spec-3.0.html) [MicroProfile Telemetry Tracing 1.0](https://download.eclipse.org/microprofile/microprofile-telemetry-1.0/tracing/microprofile-telemetry-tracing-spec-1.0.html)
*   Public URL of TCK Results Summary:

    [https://docs.payara.fish/community/docs/6.2023.10/Eclipse%20MicroProfile%20Certification/6.2023.10/Server%20TCK%20Results.html](https://docs.payara.fish/community/docs/6.2023.10/Eclipse%20MicroProfile%20Certification/6.2023.10/Server%20TCK%20Results.html)
*   Java runtime used to run the implementation:

    Zulu JDK Runtime Environment (version 11.0.14.1)
*   Summary of the information for the certification environment:

    Jenkins AWS EC2 Linux Cloud, Ubuntu 20.04 LTS\


### Test results

#### Config

```none
Completed running 368 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```

#### Fault-Tolerance

```none
Completed running 442 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```

#### Health

```none
Completed running 28 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```

#### JWT-Auth

```none
Completed running 206 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```

#### Metrics

|   | A challenge has been raised against the Metrics 5.0.0 TCK here: [https://github.com/eclipse/microprofile-metrics/issues/769](https://github.com/eclipse/microprofile-metrics/issues/769) |
| - | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

```none
Completed running 147 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```

#### OpenAPI

|   | A challenge has been raised against the OpenAPI 3.1 TCK here: [https://github.com/eclipse/microprofile-open-api/issues/557](https://github.com/eclipse/microprofile-open-api/issues/557). For the official TCK run with challenged tests excluded see "Unaltered TCK", for the Payara Patched TCK see "Payara Patched TCK". The Payara patch to the TCK is here: [https://github.com/eclipse/microprofile-open-api/pull/559](https://github.com/eclipse/microprofile-open-api/pull/559) |
| - | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

```none
Unaltered TCK
Completed running 87 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0

Payara Patched TCK
Completed running 273 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```

#### OpenTelemetry-Tracing

```none
Completed running 12 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```

#### OpenTracing

```none
Completed running 66 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```

#### Rest-Client

```none
Completed running 220 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 15

Apache HTTP Client
Completed running 5 tests
Number of tests failed 0
Number of tests with errors 0
Number of tests skipped 0
```
