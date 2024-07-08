[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Bugs](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=bugs)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Code Smells](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=code_smells)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Duplicated Lines (%)](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=duplicated_lines_density)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Lines of Code](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=ncloc)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Reliability Rating](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=reliability_rating)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Security Rating](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=security_rating)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Technical Debt](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=sqale_index)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Maintainability Rating](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=sqale_rating)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)
[![Vulnerabilities](https://sonarcloud.io/api/project_badges/measure?project=Alex-Venkov_Main-Build&metric=vulnerabilities)](https://sonarcloud.io/summary/new_code?id=Alex-Venkov_Main-Build)

# YouTube-Clone ![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white)

## Tech Stack    [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

### 1. TypeScript ![TypeScript](https://img.shields.io/badge/typescript-%23007ACC.svg?style=for-the-badge&logo=typescript&logoColor=white)

### 2. Next.js ![Nuxtjs](https://img.shields.io/badge/Nuxt-002E3B?style=for-the-badge&logo=nuxtdotjs&logoColor=#00DC82)

### 3. Express.js ![Express.js](https://img.shields.io/badge/express.js-%23404d59.svg?style=for-the-badge&logo=express&logoColor=%2361DAFB)

### 4. Docker ![Indeed](https://img.shields.io/badge/docker-003A9B?style=for-the-badge&logo=docker&logoColor=white)

### 5. FFmpeg ![Gitee](https://img.shields.io/badge/ffmpeg-C71D23?style=for-the-badge&logo=ffmpeg&logoColor=white)

### 6. Firebase Auth  ![Firebase](https://img.shields.io/badge/firebaseAuth-%23039BE5.svg?style=for-the-badge&logo=firebase)

### 7. Firebase Functions 	![Firebase](https://img.shields.io/badge/firebaseFunctions-%23039BE5.svg?style=for-the-badge&logo=firebase)

### 8. Firebase Firestore 	![Firebase](https://img.shields.io/badge/firebaseFirestore-%23039BE5.svg?style=for-the-badge&logo=firebase)

### 9. Google Cloud Storage ![Google Cloud](https://img.shields.io/badge/GoogleCloudstorage-%234285F4.svg?style=for-the-badge&logo=google-cloud&logoColor=white)

### 10. Google Cloud Pub/Sub ![Google Cloud](https://img.shields.io/badge/GoogleCloudPub/Sub-%234285F4.svg?style=for-the-badge&logo=google-cloud&logoColor=white)

### 11. Google Cloud Run ![Google Cloud](https://img.shields.io/badge/GoogleCloudRun-%234285F4.svg?style=for-the-badge&logo=google-cloud&logoColor=white)

## Architecture 

![image](https://github.com/Alex-Venkov/Main-Build/assets/97468479/635e207f-f63b-440f-a263-5e8bfe5ea908)

#### 1. Cloud Storage will store the raw and processed videos uploaded by users.

#### 2. Pub/Sub will send messages to the video processing service.

#### 3. Cloud Run will host a non-public video processing service. After it transcodes videos, they will be uploaded to Cloud Storage.

#### 4. Cloud Firestore will store the metadata for the videos.

#### 5. Cloud Run will host a Next.js app, which will serve as the Youtube web client.

#### 6. The Next.js app will make API calls to Firebase Functions.

#### 7. Firebase Functions will fetch videos from Cloud Firestore and return them.

## Requirements 

#### Users can sign in/out using their Google account

#### Users can upload videos while signed in

#### Videos should be transcoded to multiple formats (e.g. 360p, 720p)

#### Users can view a list of uploaded videos (signed in or not)

#### Users can view individual videos (signed in or not)

## Video Storage (Cloud Storage) 

####  Google Cloud Storage will be used to host the raw and processed videos. This is a simple, scalable, and cost effective solution for storing and serving large files.

## Video Upload Events (Cloud Pub/Sub)

####  When a video is uploaded, we will publish a message to a Cloud Pub/Sub topic. This will allow us to add a durability layer for video upload events and process videos asynchronously.

## Video Processing Workers (Cloud Run)

####  When a video upload event is published, a video processing worker will receive a message from Pub/Sub and transcode the video. For transcoding the video we will use ffmpeg, which is a popular open source tool for video processing and it's widely used in industry (including at YouTube).

The nature of video processing can lead to inconsistent workloads, so we will use Cloud Run to scale up and down as needed. Processed videos will be uploaded back to Cloud Storage.

## Video Metadata (Firestore) 

#### Video Metadata (Firestore)
After a video is processed, we will store the metadata in Firestore. This will allow us to display processed videos in the web client along with other relevant info (e.g. title, description, etc).

## Video API (Firebase Functions)

####  We will use Firebase Functions to build a simple API that will allow users to upload videos and retrieve video metadata. This can easily be extended to support additional Create, Read, Update, Delete (CRUD) operations.

## Web Client (Next.js / Cloud Run)

####  We will use Firebase Functions to build a simple API that will allow users to upload videos and retrieve video metadata. This can easily be extended to support additional Create, Read, Update, Delete (CRUD) operations.

## Authentication (Firebase Auth)

####  We will use Firebase Auth to handle user authentication. This will allow us to easily integrate with Google Sign In.

# Detailed Design

## 1. User Sign Up

Users can sign up using their Google account and this easily handled by Firebase Auth. A user record will be created including a unique auto-generated ID for the user, as well as the user's email address.

For us to include additional info about the user we will create a Firestore document for each user, which will be apart of the users collection. This will allow us to store additional info about the user (e.g. name, profile picture, etc).

Firebase Auth is mainly integrated into client code, which in our case will be within our Next.js app. While it's possible to trigger a user document creation directly from the client (after a new user signs up), this approach has some error prone edge cases.

What if there is an network issue or the user's browser crashes right after the user signs up but before the user document is created?

Fortunately, Firebase Auth provides triggers so that we don't have to rely on the client for this operation. We can just trigger a Firebase Function (i.e. a server-side endpoint) to create the user document whenever a new user is created.


## 2. Video Upload

Ideally, we should only allow authenticated users to upload videos. This will allow us to associate the uploaded video with the user who uploaded it. In the future this could also allow us to enforce quotas on video uploads (e.g. 10 videos per day).

While we could allow users to upload videos directly to a server we manage ourselves, it's more simple to use a service like Google Cloud Storage, which is specifically designed for arbitrarily large files like videos.

But to prevent unauthorized users from uploading videos, we will generate a signed URL that will allow the user to upload a video directly to Cloud Storage. We will implement this in a public Firebase Function, but this way we can easily ensure the user is authenticated before generating the URL.

## 2. Video Processing

We'd like to process videos as soon as they come in, but it's possible that we could receive a large number of uploads at once which we can't immediately process. To solve this problem we will introduce a message queue to our system - Cloud Pub/Sub.

This provides many benefits:

#### 1. When a video is uploaded to Cloud Storage, we can publish a message to a Pub/Sub topic. This will allow us to decouple the video upload from the video processing.

#### 2. We will use a Pub/Sub subscriptions to push messages to our video processing workers. In the future we can add additional subscriptions to fan-out these messages, e.g. for analytics.

#### 3. If the workers don't have enough capacity to process all the messages, Pub/Sub will automatically buffer the messages for us. This will allow us to scale our workers up and down as needed.

After a video is processed, we will upload it to a public Cloud Storage bucket. We will also store the video metadata in Firestore, including the video's processing status. This will allow us to display the processed videos in the web client.

There are some noteworthy limitations with this approach, such as:

#### 1. A Cloud Run request has a max timeout of 3600 seconds

#### 2. Pub/Sub will redeliver a message after at most 600 seconds, closing the previous HTTP connection

#### 3. We don't check for illegal content within videos

# References

## Firebase Auth: https://firebase.google.com/docs/auth

## Cloud Storage Signed URLs: https://cloud.google.com/storage/docs/access-control/signed-urls

## Pub/Sub Push subscriptions: https://cloud.google.com/pubsub/docs/push

## Using Pub/Sub with Cloud Storage: https://cloud.google.com/storage/docs/pubsub-notifications

## Using Pub/Sub with Cloud Run: https://cloud.google.com/run/docs/tutorials/pubsub

## The main idea and the author : https://neetcode.io 

# Advanced Limitations 

## 1. Long Lived HTTP Requests

For Pub/Sub the max ack deadline is 600 seconds, but the max request processing time for Cloud Run is 3600 seconds.

We ensured that a video won't be processed multiple times by syncing the video status in Firestore.

But, if the video processing takes longer than 600 seconds, Pub/Sub will close the HTTP connection. Now while the video will still be processed by out service, we will never be able to send the ack to Pub/Sub. This means the message will be stuck in the Pub/Sub queue forever, and will continuously be redelivered to our service.

So how can we fix this?

We can use the Pull Subscription method instead of the Push Subscription method. This way we can control when we want to pull messages from the Pub/Sub queue. We can pull a message, process it, and then send the ack to Pub/Sub.

After we pull a message it is temporarily removed from the queue. If we don't send the ack within the ack deadline (still a max of 10 minutes), the message will be redelivered to the queue.

But since we are still syncing the status with Firestore, we won't reproduce the video.

By pulling messages, we can ensure that no message will be stuck forever in the Pub/Sub queue.

## 2. Video Processing Failure

If we pull a message from the Pub/Sub queue, change the status of it to processing in Firestore, and then our video processing service fails, the message will be stuck in the Pub/Sub queue forever.

We could send an ack as soon as we pull the message from the queue, but then what would happen if we fail to process the video? We would have to somehow requeue the message. The easiest solution would be to reset the status to undefined.

## 3. File Upload Time Limit

This is actually not a limitation. You might think that since the signed URL we generate is only valid for 15 minutes, that if we have a slow internet connection, we might not be able to upload the video in time.

But as long as we start the upload within 15 minutes, the upload will continue even after the signed URL expires. This is because the signed URL is only used to authenticate the upload request. After the upload request is authenticated, the upload will continue until it is completed.

#### If anything, we should shorten the time limit for the signed URL to 1 minute.

## 4. File Upload Time Limit

Google Cloud Storage provides a basic level of file streaming for videos. But it is not optimized for video streaming. This is good because we don't have to download the entire video before we can start watching it.

But it still isn't nearly as powerful as Youtube's custom video streaming / chunking solution.

Youtube uses DASH (Dynamic Adaptive Streaming over HTTP) to stream videos. This allows them to serve different quality videos to users based on their internet connection. It also allows them to serve the video in chunks, so that the user doesn't have to download the entire video before they can start watching it.

Another popular alternative is HLS (HTTP Live Streaming). This is what Netflix uses.

#### I believe Youtube also uses HLS in some capacity.

## 5. File Upload Time Limit

Ideally, we would want to serve our videos from a CDN (Content Delivery Network). This would allow us to serve our videos from a server that is geographically close to the user. This would reduce latency and improve the user experience.

## 6. Illegal Videos

We have no way of checking if a video is illegal before serving it to users. This is a big problem. For this reason, it may be unsafe to deploy this app publically.








