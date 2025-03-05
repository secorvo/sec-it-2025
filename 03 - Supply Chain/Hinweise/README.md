# Hinweise zu Secure-Software-Supply-Chain

## syft

### Erzeugen eines SBOM aus einem lokalen Docker-Containers

```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/d/TEMP:/tmp --name Syft anchore/syft:latest scan traefik -o cyclonedx-xml=/tmp/traefik.xml

docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/d/TEMP:/tmp --name Syft anchore/syft:latest scan traefik -o cyclonedx-json=/tmp/traefik.json
```

## grype

```bash
docker run --rm -v /mnt/d/TEMP:/tmp --name Grype anchore/grype:latest sbom:/tmp/juice.xml

 docker run --rm -v /var/run/docker.sock:/var/run/docker.sock --name Grype anchore/grype:latest deciduous
 ```

## trivy

[trivy Documentation](https://trivy.dev/latest/docs/)
[trivy Github Repo](https://github.com/aquasecurity/trivy)

```bash
docker run -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/Library/Caches:/root/.cache/ aquasec/trivy:latest image python:3.4-alpine

docker run -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/Library/Caches:/root/.cache/ -v /mnt/d/TEMP:/tmp aquasec/trivy:latest sbom /tmp/traefik.json
```

## OWASP Dependency Track

[Dependency Track Github Repo](https://github.com/DependencyTrack)
[Dependency Track Homepage](https://dependencytrack.org)

### Installation

```bash
# Downloads the latest Docker Compose file
curl -LO https://dependencytrack.org/docker-compose.yml

# Starts the stack using Docker Compose
docker-compose up -d
```

### Upload eines SBOM via API

```bash

 export TRAEFIK_JSON_PROJECT_ID=383c9877-a845-45c6-846e-67c1b8bc0106
 export TRAEFIK_XML_PROJECT_ID=e525b63c-d853-40c9-be18-9d03dfdef6b6
 export JUICE_XML_PROJECT_ID=

export DT_SBOM_FILE=<file>
export DT_API_KEY=<api_key>
export DT_PROJECT_ID=<project_uuid>

curl -X "POST" "http://localhost:8081/api/v1/bom" \
     -H 'Content-Type: multipart/form-data' \
     -H "X-Api-Key: $DT_API_KEY" \
     -F "project=$DT_PROJECT_IT" \
     -F "bom=@$DT_SBOM_FILE"
```
