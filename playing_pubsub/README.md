# Cloud Run Pub/Sub Tutorial Sample

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
````shell
gcloud pubsub topics create my-topic

gcloud pubsub subscriptions create my-sub-event-test --topic my-topic \
--ack-deadline=60

gcloud pubsub topics list

gcloud pubsub subscriptions list

gcloud pubsub topics publish my-topic --message hello

gcloud pubsub topics publish my-topic --message goodbye

gcloud pubsub subscriptions pull --auto-ack --limit=2 my-sub

gcloud pubsub subscriptions ack my-sub --ack-ids ACK_ID

gcloud storage buckets notifications create \
gs://my-bucket-event-test --topic=my-topic

````

--
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

gcloud pubsub topics create myRunTopic

Download code: https://github.com/GoogleCloudPlatform/java-docs-samples/tree/main/run/pubsub

Update pom.xml with the project name.

gcloud auth configure-docker
mvn compile jib:build -D image=gcr.io/fichaje-kt/pubsub


gcloud run deploy pubsub-tutorial --image gcr.io/fichaje-kt/pubsub  --no-allow-unauthenticated


gcloud iam service-accounts create cloud-run-pubsub-invoker \
--display-name "Cloud Run Pub/Sub Invoker"



gcloud run services add-iam-policy-binding pubsub-tutorial \
--member=serviceAccount:cloud-run-pubsub-invoker@fichaje-kt.iam.gserviceaccount.com \
--role=roles/run.invoker


gcloud projects add-iam-policy-binding fichaje-kt \
--member=serviceAccount:service-747954516128@gcp-sa-pubsub.iam.gserviceaccount.com \
--role=roles/iam.serviceAccountTokenCreator



gcloud pubsub subscriptions create myRunSubscription --topic myRunTopic \
--ack-deadline=600 \
--push-endpoint=https://pubsub-tutorial-k3biuuszha-no.a.run.app/ \
--push-auth-service-account=cloud-run-pubsub-invoker@fichaje-kt.iam.gserviceaccount.com


gcloud pubsub subscriptions delete myRunSubscription