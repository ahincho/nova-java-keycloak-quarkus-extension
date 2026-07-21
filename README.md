# nova-java-keycloak-quarkus-extension

> Quarkus extension for the Nova Keycloak core library. Auto-wires JWT parsing
> and profile extraction under the `nova.keycloak.*` configuration prefix.

[![Nova Platform](https://img.shields.io/badge/Nova-Platform-00add8)](https://github.com/ahincho/nova-platform)
[![Quarkus](https://img.shields.io/badge/Quarkus-3.33.x%20LTS-4695eb)](https://quarkus.io/)
[![Java](https://img.shields.io/badge/Java-25-b07219)](https://openjdk.org/projects/jdk/25/)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue)](LICENSE)

## What it is

A Quarkus extension that consumes `nova-java-keycloak` and exposes its facade
through CDI:

- `@Singleton` `KeycloakProfileResolver` bean for the current request profile,
- `KeycloakConfig` `@ConfigMapping` under the `nova.keycloak.*` prefix
  (issuer URI, realm, client id, JWKS refresh interval, ...),
- `META-INF/jandex.idx` so Quarkus build-time scan picks up beans without
  reflection at runtime.

## When to use

In a Quarkus 3.33.x LTS application that authenticates against a Keycloak realm
and needs to read public JWT claims (`preferred_username`, roles, ...) in a
typed way without hand-rolling JWK fetching / claim extraction.

```xml
<dependency>
  <groupId>io.github.ahincho</groupId>
  <artifactId>nova-java-keycloak-quarkus-extension</artifactId>
  <version>0.1.0-SNAPSHOT</version>
</dependency>
```

```properties
# application.properties
nova.keycloak.issuer-uri=https://keycloak.example.com/realms/my-realm
nova.keycloak.client-id=my-app
```

```java
@Inject KeycloakProfileResolver profiles;

@GET
@Path("/me")
public KeycloakProfile me(@HeaderParam("Authorization") String bearer) {
    return profiles.resolve(bearer);
}
```

## Status

> **Work in progress.** Coordinates and config keys are not final.

## Related

- `nova-java-keycloak` — pure-Java core that this extension wraps.
- `nova-java-quarkus-parent` — parent POM aggregating Nova Quarkus
  dependencies and plugins.

## Build

```bash
./mvnw clean verify
```

Requires **JDK 25** and **Maven 3.9+** (the extension is built with the
Quarkus extension plugin).

## License

Apache License 2.0. See [LICENSE](LICENSE).
