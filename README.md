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

| Parameter | Description | Default |
|-------------------------------------------|-------------------------------------------------------|----------------------------------|
| replicaCount | Number of replicas | 1 |
| image.web.repository | Web image repository | chiefonboarding/chiefonboarding|
| image.web.tag | Web image tag | latest |
| image.web.pullPolicy | Web image pull policy | IfNotPresent |
| image.db.repository | Database image repository | postgres |
| image.db.tag | Database image tag | latest |
| image.db.pullPolicy | Database image pull policy | IfNotPresent |
| service.web.type | Service type for web | ClusterIP |
| service.web.port | Service port for web | 8000 |
| service.db.type | Service type for database | ClusterIP |
| service.db.port | Service port for database | 5432 |
| ingress.enabled | Enable ingress | false |
| ingress.className | Ingress class name | internal |
| ingress.annotations | Ingress annotations | [] |
| ingress.hosts.host | Ingress host | chiefonboarding.local |
| ingress.hosts.paths.path | Ingress paths | / |
| ingress.hosts.paths.pathType | Ingress path type | ImplementationSpecific |
| ingress.tls.hosts | Ingress TLS hosts | chiefonboarding.local |
| ingress.tls.secretName | Ingress TLS secret name | mysupersecret |
| postgresql.postgresDatabase | PostgreSQL database name | chiefonboarding |
| postgresql.internal.enabled | Internal PostgreSQL enabled | true |
| postgresql.internal.persistence.enabled | Internal PostgreSQL persistence enabled | true |
| postgresql.internal.persistence.accessMode | Internal PostgreSQL persistence access mode | ReadWriteOnce |
| postgresql.internal.persistence.size | Internal PostgreSQL persistence size | 10Gi |
| postgresql.external.enabled | External PostgreSQL enabled | false |
| postgresql.external.host | External PostgreSQL host | remote.db.uri |
| postgresql.external.port | External PostgreSQL port | 5432 |
| postgresql.credentials.externalSecret.enabled | External PostgreSQL credentials from secret | false |
| postgresql.credentials.externalSecret.secretName | PostgreSQL credentials secret name | external-postgres-secret |
| postgresql.credentials.externalSecret.secretUserKey | Secret user key for PostgreSQL credentials | postgres-username |
| postgresql.credentials.externalSecret.secretPasswordKey | Secret password key for PostgreSQL credentials | postgres-password |
| postgresql.credentials.postgresUser | PostgreSQL user | "" |
| postgresql.credentials.postgresPassword | PostgreSQL password | "" |
| settings.secretKey | Secret key | mysupesecretkey |
| settings.apiAccess | API access | true | 
| settings.baseUrl | Base URL | http://localhost |
| settings.allowedHost | Allowed hosts | '*' |
| settings.objectStorage.enabled | Object storage enabled | false |
| settings.objectStorage.awsS3EndpointUrl | AWS S3 endpoint URL | "" |
| settings.objectStorage.awsStorageBucketName | AWS storage bucket name | "" |
| settings.objectStorage.awsDefaultRegion | AWS default region | "" |
| settings.objectStorage.awsCredentialsSecret | AWS credentials secret | "" |
| settings.email.enabled | Email enabled | true |
| settings.email.emailHost | Email host | smtp.chiefonboarding.com |
| settings.email.emailPort | Email port | 587 |
| settings.email.emailUser | Email user | my-super-secret |
| settings.email.emailPassword | Email password | password |
| settings.email.emailUseTls | Use TLS for email | False |
| settings.email.emailUseSsl | Use SSL for email | True |
| settings.email.externalSecret.enabled | External email secret enabled | false |
| settings.email.externalSecret.emailSecretName | Email credentials secret name | external-postgres-secret |
| settings.email.externalSecret.emailSecretUserKey | Secret user key for email credentials | email_user_key |
| settings.email.externalSecret.emailSecretPasswordKey | Secret password key for email credentials | email_password_key |
| settings.email.emailUser | Email user | "" |
| settings.email.emailPassword | Email password | "" |
| settings.twilio.enabled | Twilio integration enabled | false |
| settings.twilio.twilioFromNumber | Twilio from number | XXXXXXXXX |
| settings.twilio.externalSecret | External Twilio secret enabled |false | 
| settings.twilio.externalSecret.twilioSecretName | Twilio credentials secret name  | "" | 
| settings.twilio.externalSecret.twilioSecretAccountSid | Secret Account SID for Twilio credentials  | "" | 
| settings.twilio.externalSecret.twilioSecretAccountToken | Secret Account Token for Twilio credentials | "" | 
| settings.twilio.twilioAccountSid | Twilio Account SID  | "" |


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

## Secrets
For testing, you can use clear text credentials for services such as postgresql and smtp. This is not recommended for production. Use instead a secret to read the values from.

## Notes
Adjust the values.yaml file according to your environment and requirements.
Ensure that your Kubernetes cluster has access to the Docker images specified in the values.yaml file.
