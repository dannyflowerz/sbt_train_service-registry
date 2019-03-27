# service-registry

Service registry for the microservices

## GCP

### Create bucket for all deployments
```
gsutil mb gs://sbt-train-bucket
```

### Upload jar file to bucket
```
gsutil cp target/service-registry-0.0.1-SNAPSHOT.jar \
    gs://sbt-train-bucket/service-registry.jar
```

### Create compute instance for service
```
gcloud compute instances create service-registry-instance \
--image-family debian-9 \
--image-project debian-cloud \
--machine-type g1-small \
--scopes "userinfo-email,cloud-platform" \
--metadata-from-file startup-script=instance-startup.sh \
--metadata BUCKET=sbt-train-bucket \
--zone europe-west2-a \
--tags service-registry
```

### Crete firewall rule to allow access on 8761
```
gcloud compute firewall-rules create default-allow-http-8761 \
    --allow tcp:8761 \
    --source-ranges 0.0.0.0/0 \
    --description "Allow port 8761 access to all targets"
```

### Stop instance to avoid extra costs
```
gcloud compute instances stop service-registry-instance
```
