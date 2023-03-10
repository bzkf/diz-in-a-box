# diz-in-a-box

Datenintegrationszentrum in einer Box.

![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square)

## Installing the Chart

To install the chart with the release name `my-diz`:

```console
helm install my-diz oci://ghcr.io/bzkf/diz-in-a-box/charts/diz-in-a-box
```

## Requirements

Kubernetes: `>= 1.21.0`

| Repository                                             | Name                | Version |
| ------------------------------------------------------ | ------------------- | ------- |
| https://akhq.io/                                       | akhq                | 0.3.1   |
| https://hapifhir.github.io/hapi-fhir-jpaserver-starter | hapi-fhir-jpaserver | 0.11.0  |
| https://miracum.github.io/charts                       | stream-processors   | 1.1.9   |

## Values

| Key                                                                                    | Type   | Default                                      | Description                                                                                                                                                   |
| -------------------------------------------------------------------------------------- | ------ | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| akhq.configuration.akhq.connections.bzkf-dizbox-cluster.properties."bootstrap.servers" | string | `"bzkf-dizbox-cluster-kafka-bootstrap:9092"` | the Kafka bootstrap server. Needs to be changed if the chart release name is changed from the default `bzkf-dizbox`                                           |
| akhq.enabled                                                                           | bool   | `true`                                       | whether the included [Kafka UI AKHQ](https://akhq.io/) should be installed                                                                                    |
| hapi-fhir-jpaserver.enabled                                                            | bool   | `true`                                       | whether the included [HAPI FHIR JPA Server](https://github.com/hapifhir/hapi-fhir-jpaserver-starter) should be installed                                      |
| hapi-fhir-jpaserver.postgresql.auth.postgresPassword                                   | string | `"fhir"`                                     | the postgres database root (`postgres`) user. this should be changed for improved security.                                                                   |
| hapi-fhir-jpaserver.postgresql.primary.persistence.size                                | string | `"32Gi"`                                     | size for the HAPI FHIR server's PostgreSQL database                                                                                                           |
| stream-processors.enabled                                                              | bool   | `true`                                       | whether the included [stream processors](https://github.com/miracum/charts/tree/master/charts/stream-processors) chart should be installed                    |
| stream-processors.strimziClusterName                                                   | string | `"bzkf-dizbox-cluster"`                      | name of the Kafka cluster deployed by the Strimzi Operator. This should be the same as the name in the [Kafka custom resource](../../k8s/kafka-cluster.yaml). |

---

Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)
