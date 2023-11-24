# obds-to-fhir Docker Compose Version

## Run only the obds-to-fhir job

```sh
docker compose -f compose.obds-to-fhir.yaml up
```

## Run while also starting a Kafka cluster and Kafka connect

```sh
docker compose -f compose.obds-to-fhir.yaml -f compose.full.yaml up
```

Open <http://localhost:8084/> to view the cluster's topics.

## Load sample data from a oBDS Sammelmeldung into the Kafka cluster

```sh
docker compose -f compose.decompose-xmls.yaml up
```

## Convert the FHIR resources to a CSV dataset

```sh
sudo chown -R 1001:1001 ./opal-output/
docker compose -f compose.obds-fhir-to-opal.yaml up
```

## Start the entire stack

```sh
docker compose -f compose.obds-to-fhir.yaml -f compose.full.yaml -f compose.decompose-xmls.yaml -f compose.obds-fhir-to-opal.yaml up
```

## Enable Kafka Connect and the connector

Make sure to have access to Onkostar tables `lkr_meldung`, `lkr_meldung_export` and `erkrankung`.

The following SQL query will SELECT required information with columns filtered by type and containing required ICD_Version entry in `XML_DATEN`:

```sql
SELECT * FROM (
  SELECT YEAR(e.diagnosedatum) AS YEAR, versionsnummer AS VERSIONSNUMMER, lme.id AS ID, CONVERT(lme.xml_daten using utf8) AS XML_DATEN
    FROM lkr_meldung_export lme
    JOIN lkr_meldung lm ON lme.lkr_meldung = lm.id
    JOIN erkrankung e ON lm.erkrankung_id = e.id
    WHERE typ != '-1' AND versionsnummer IS NOT NULL AND lme.XML_DATEN LIKE '%ICD_Version%'
) o;
```

```sh
docker compose -f compose.obds-to-fhir.yaml -f compose.full.yaml up
```

```sh
curl -X POST \
  -H 'Content-Type: application/json' \
  -d @onkostar-db-connector.json \
  http://localhost:8083/connectors
```

## Run with enabled pseudonymization

> **Warning**
> Requires gPAS to be set-up and the [anonymization.yaml](anonymization.yaml) to be configured

```sh
docker compose -f compose.obds-to-fhir.yaml -f compose.full.yaml -f compose.pseudonymization.yaml up
```

## Run with enabled pseudonymization and sending resources to a FHIR server

```sh
docker compose -f compose.obds-to-fhir.yaml -f compose.full.yaml -f compose.fhir-server.yaml -f compose.pseudonymization.yaml up
```
