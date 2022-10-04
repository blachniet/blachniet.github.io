---
title: "Authenticate Cloud Vision Client in Go"
date: "2017-05-28"
categories:
- "site-updates"
tags:
- "go"
- "google-cloud-platform"
aliases:
- "/blog/auth-cloud-vision-go/"
blachnietWordPressExport: true
---

[Google's Cloud Platform](https://cloud.google.com/) documentation is top of the line. Even with the documentation though, it took me some time to figure out how to use the [Cloud Vision API](https://cloud.google.com/vision/) from a Go application running on a [Compute Engine](https://cloud.google.com/compute/) instance. All the right information is there, but it's scattered. Below I've compiled this information into a few steps.

Authenticating with the Cloud Vision API involves more setup than the Cloud Datastore, Cloud Pub/Sub and Cloud Storage. The client libraries for the latter use  [Google Application Default Credentials](https://developers.google.com/identity/protocols/application-default-credentials) and "just work" when running on a Compute Engine instance. In order to authenticate with the Cloud Vision API, you need to create a service account key and instruct the client library to use that key when sending API requests. See [Authenticating to the Cloud Vision API](https://cloud.google.com/vision/docs/auth#using_a_service_account) for more details.

### Prerequisites

- You have a [Google Cloud project](https://console.cloud.google.com/home/dashboard)
- You have a [Compute Engine](https://console.cloud.google.com/compute/instances) instance and a service account associated with it (e.g. 123456789-compute@developer.gserviceaccount.com)
- You have a Go app that uses the [Cloud Vision API client library](https://cloud.google.com/vision/docs/reference/libraries)

### Steps

1. Enable the Cloud Vision API for your project: [https://console.cloud.google.com/apis/api/vision.googleapis.com/overview](https://console.cloud.google.com/apis/api/vision.googleapis.com/overview)
2. If you haven't already, create a JSON key for the service account running the Compute Engine instance: [https://console.cloud.google.com/iam-admin/serviceaccounts/project](https://console.cloud.google.com/iam-admin/serviceaccounts/project). Click the dropdown beside the service account, click **Create Key** and select the **JSON** key type.
3. Copy the key JSON file to your Compute Engine instance
4. When creating the Vision API client in Go, provide the service account file with `option.WithServiceAccountFile("/path/to/key.json")`:
    
    ```
    visionClient, err := vision.NewImageAnnotatorClient(ctx, option.WithServiceAccountFile("/path/to/key.json"))
        if err != nil {
            return nil, nil, errors.Wrap(err, "Error creating vision client")
        }
    ```
