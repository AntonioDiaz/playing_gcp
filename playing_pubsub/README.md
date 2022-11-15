# Cloud Run Pub/Sub Tutorial Sample

- [Dependencies](#dependencies)
- [My notes](#my-notes)
- [GCP - Cloud Run & Pub/Sub](#gcp---cloud-run--pubsub)

This sample shows how to create a service that processes Pub/Sub messages.

Use it with the [Cloud Pub/Sub with Cloud Run tutorial](http://cloud.google.com/run/docs/tutorials/pubsub).

For more details on how to work with this sample read the [Google Cloud Run Java Samples README](https://github.com/GoogleCloudPlatform/java-docs-samples/tree/main/run).

[![Run in Google Cloud][run_img]][run_link]

[run_img]: https://storage.googleapis.com/cloudrun/button.svg
[run_link]: https://deploy.cloud.run/?git_repo=https://github.com/GoogleCloudPlatform/java-docs-samples&dir=run/pubsub


## Dependencies

* **Spring Boot**: Web server framework.
* **Jib**: Container build tool.
* **Junit**: [development] Test running framework.


## My notes

Cloud Run Image processing:  https://cloud.google.com/run/docs/tutorials/image-processing

--
* create topic
```shell
gcloud pubsub topics create my-topic
```

* Create subscription for the topic
```shell
gcloud pubsub subscriptions create my-sub-event-test --topic my-topic \
--ack-deadline=60
```

* List all topics
```shell
gcloud pubsub topics list
```

* List all subscriptions
```shell
gcloud pubsub subscriptions list
```

* Send message to the topic
```shell
gcloud pubsub topics publish my-topic --message hello
gcloud pubsub topics publish my-topic --message goodbye
```

* Read messages through a subscription
```shell  
gcloud pubsub subscriptions pull --auto-ack --limit=2 my-sub
```

gcloud pubsub subscriptions ack my-sub --ack-ids ACK_ID

* Create a topic associate to a bucket
```shell
gcloud storage buckets notifications create \
  gs://my-bucket-event-test --topic=my-topic
```


* Example of an event message of create a file on a bucket
````json
{
    "kind": "storage#object",
    "id": "my-bucket-event-test/mongo.jpeg/1667591803688718",
    "selfLink": "https://www.googleapis.com/storage/v1/b/my-bucket-event-test/o/mongo.jpeg",
    "name": "mongo.jpeg",
    "bucket": "my-bucket-event-test",
    "generation": "1667591803688718",
    "metageneration": "1",
    "contentType": "image/jpeg",
    "timeCreated": "2022-11-04T19:56:43.793Z",
    "updated": "2022-11-04T19:56:43.793Z",
    "storageClass": "STANDARD",
    "timeStorageClassUpdated": "2022-11-04T19:56:43.793Z",
    "size": "52508",
    "md5Hash": "ybnLTTRT9RSThuDCl6UqeQ==",
    "mediaLink": "https://storage.googleapis.com/download/storage/v1/b/my-bucket-event-test/o/mongo.jpeg?generation=1667591803688718&alt=media",
    "crc32c": "YXLU1g==",
    "etag": "CI6u28inlfsCEAE="
}
````




gcloud iam service-accounts create my-invoker-service-account-name \
--display-name "My Invoker Service Account"

gcloud run services add-iam-policy-binding my-invoker-service-account-name \
--member=serviceAccount:SERVICE_ACCOUNT_NAME@fichaje-kt.iam.gserviceaccount.com \
--role=roles/run.invoker




https://cloud.google.com/run/docs/tutorials/pubsub#run_pubsub_dockerfile-java


## GCP - Cloud Run & Pub/Sub

https://cloud.google.com/run/docs/tutorials/pubsub

* Create topic
```shell
gcloud pubsub topics create myRunTopic
```


Download code: https://github.com/GoogleCloudPlatform/java-docs-samples/tree/main/run/pubsub

Update pom.xml with the project name.

```shell
gcloud auth configure-docker
mvn compile jib:build -D image=gcr.io/fichaje-kt/pubsub
```

```shell
gcloud run deploy pubsub-tutorial --image gcr.io/fichaje-kt/pubsub \
--no-allow-unauthenticated
```

```shell
gcloud iam service-accounts create cloud-run-pubsub-invoker \
--display-name "Cloud Run Pub/Sub Invoker"
```

```shell
gcloud run services add-iam-policy-binding pubsub-tutorial \
--member=serviceAccount:cloud-run-pubsub-invoker@fichaje-kt.iam.gserviceaccount.com \
--role=roles/run.invoker
```

```shell
gcloud projects add-iam-policy-binding fichaje-kt \
--member=serviceAccount:service-747954516128@gcp-sa-pubsub.iam.gserviceaccount.com \
--role=roles/iam.serviceAccountTokenCreator
```shell

```shell
gcloud pubsub subscriptions create myRunSubscription --topic myRunTopic \
--ack-deadline=600 \
--push-endpoint=https://pubsub-tutorial-k3biuuszha-no.a.run.app/ \
--push-auth-service-account=cloud-run-pubsub-invoker@fichaje-kt.iam.gserviceaccount.com
```

```shell
gcloud pubsub subscriptions delete myRunSubscription
```

```json
{
    "message": {
        "data": "string",
        "attributes": {
            "string": "string"
        },
        "messageId": "string",
        "publishTime": "string",
        "orderingKey": "string"
    }
}
```