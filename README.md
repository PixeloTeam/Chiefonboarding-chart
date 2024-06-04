# ChiefOnboarding helm chart

This Helm chart deploys the ChiefOnboarding application along with a PostgreSQL database. Chiefonboarding is an application for automating onboardings and provisioning accounts by creating pipelines. The users can interact with the app through a Slack bot. Project website: https://chiefonboarding.com/

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+

## Installation

To install the chart with the release name `my-release`:

```sh
helm install my-release chiefonboarding-0.1.0.tgz
```

The command deploys ChiefOnboarding on the Kubernetes cluster with the default configuration. The values.yaml file can be customized to override the default settings.

## Uninstallation
To uninstall/delete the my-release deployment:

```sh
helm uninstall my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration
The following table lists the configurable parameters of the Helm chart and their default values.

| Parameter                         | Description                                                   | Default                         |
|-----------------------------------|---------------------------------------------------------------|---------------------------------|
| `replicaCount`                    | Number of replicas for both web and db deployments            | `1`                             |
| `image.web.repository`            | Web application Docker image repository                       | `chiefonboarding/chiefonboarding` |
| `image.web.tag`                   | Web application Docker image tag                              | `latest`                        |
| `image.web.pullPolicy`            | Web application Docker image pull policy                      | `IfNotPresent`                  |
| `image.db.repository`             | Database Docker image repository                              | `postgres`                      |
| `image.db.tag`                    | Database Docker image tag                                     | `latest`                        |
| `image.db.pullPolicy`             | Database Docker image pull policy                             | `IfNotPresent`                  |
| `service.web.type`                | Web service type                                              | `ClusterIP`                     |
| `service.web.port`                | Web service port                                              | `8000`                          |
| `service.db.type`                 | Database service type                                         | `ClusterIP`                     |
| `service.db.port`                 | Database service port                                         | `5432`                          |
| `ingress.enabled`                 | Enable ingress for web component                              | `true`                          |
| `ingress.className`               | Ingress class name                                            | `""`                            |
| `ingress.annotations`             | Annotations for the ingress                                   | `{}`                            |
| `ingress.hosts`                   | Hosts configuration for ingress                               | `[{ host: localhost, paths: [{ path: /, pathType: ImplementationSpecific }] }]` |
| `ingress.tls`                     | TLS configuration for ingress                                 | `[]`                            |
| `postgresql.internal.enabled`     | Enable internal PostgreSQL deployment                         | `true`                          |
| `postgresql.internal.postgresDatabase` | PostgreSQL database name                                | `chiefonboarding`               |
| `postgresql.internal.postgresUser`| PostgreSQL username                                           | `postgres`                      |
| `postgresql.internal.postgresPassword` | PostgreSQL password                                      | `securepassword123`             |
| `postgresql.external.enabled`     | Enable external PostgreSQL service                            | `false`                         |
| `postgresql.external.host`        | External PostgreSQL host                                      | `""`                            |
| `postgresql.external.port`        | External PostgreSQL port                                      | `5432`                          |
| `postgresql.external.secretName`  | Secret name for external PostgreSQL credentials               | `external-postgres-secret`      |
| `postgresql.external.secretUserKey` | Secret key for PostgreSQL username                         | `postgres-username`             |
| `postgresql.external.secretPasswordKey` | Secret key for PostgreSQL password                    | `postgres-password`             |
| `secretKey`                       | Secret key for the web application                            | `somethingsupersecret`          |
| `persistence.enabled`             | Enable persistence for PostgreSQL data                        | `true`                          |
| `persistence.accessMode`          | Access mode for persistence                                   | `ReadWriteOnce`                 |
| `persistence.size`                | Size of the persistence volume                                | `10Gi`                          |


```sh
helm install my-release --set secretKey=mysecretkey chiefonboarding-0.1.0.tgz
```
Alternatively, you can create a values.yaml file with the desired configuration and use it during installation:

```sh
helm install my-release -f values.yaml chiefonboarding-0.1.0.tgz
```

## Persistence
The PostgreSQL database uses a PersistentVolumeClaim to store data. If persistence is enabled, ensure that the storage class you are using supports dynamic provisioning or create the PersistentVolume manually.

## Ingress
To enable ingress, set ingress.enabled to true. You can customize the ingress configuration using the values.yaml file.

## Notes
Adjust the values.yaml file according to your environment and requirements.
Ensure that your Kubernetes cluster has access to the Docker images specified in the values.yaml file.
