# Configuring an Object Storage Backup Location (MinIO)

For this test drive, we are going to configure a local S3-compatible object storage system using MinIO in order to use as storage for backup data.

MinIO is a High Performance Object Storage released under Apache License v2.0. It is API compatible with Amazon S3 cloud storage service.

Create a namespace minio

```
kubectl create namespace minio
```

Configure MinIO Helm repo

```
helm repo add minio https://helm.min.io
```

Install the Chart

```
helm install minio minio/minio --namespace=minio --version=8.0.0 --set accessKey="AKIAIOSFODNN7EXAMPLE",secretKey="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",defaultBucket.enabled=true,defaultBucket.name=kanister --wait --timeout 5m
```

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/db6695d0-3e8a-4d57-bd01-6d3a4f655fd8)


## Configure a Location Profile
Next, we are going to create a Location Profile using the object storage system we configured in the previous step.

Next, we are going to create a Location Profile using the object storage system we configured in the previous step.

- Click on K10 Dashboard tab in the upper left corner
- Click Settings
- From the left click Locations
- Click New Profile
- Enter the profile name k10
- Select S3 Compatible
- Fill out the fields with the following key information:
  - S3 Access Key: AKIAIOSFODNN7EXAMPLE
  - S3 Secret Key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
  - Endpoint: http://minio.minio.svc.cluster.local:9000
  - Bucket Name: kanister

