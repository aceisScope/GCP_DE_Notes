# Compute

## [Compute Engine](https://cloud.google.com/compute/)

Example

gcloud compute instances create my-vm –custom-cpu 4 –custom-memory 5

gcloud compute instances create my-vm –zone us-central1-b –preemptible

Features:

-   predefined machine types up to 160 vCPUs and 3.75 TB of memory
-   custom machine types
-   persistent disk (64 TB in size)
    -   ssd or hdd
    -   can take snapshots and create new persistent disks
-   local ssd for scratch storage (3TB in size)
-   debian, centos, coreos, suse, ubuntu, etc images including bring
    your own
-   batch processing (preemptible vms)
-   encryption
-   per second billing
-   committed use discounts

Premptible instances can run a custom shutdown script. Instance is given
30 seconds.

## [App Engine](https://cloud.google.com/appengine/)

Platform as a Service (PaaS).

Features:

-   Languages: javascript, ruby, go, .net, java, python, and php
-   fully managed
-   monitoring through stackdriver
-   easy app versioning
-   traffic splitting A/B tests and incremental features
-   ssl/tls managed
-   integrated with GCP services

## [Kubernetes Engine](https://cloud.google.com/kubernetes-engine/)

Hosted kubernetes engine. Super simple to get started a Kubernetes
cluster.

Features:

-   integrated with Identify & Access management
-   hybrid networking (IP Address for clusters to coexist with private
    ips)
-   HIPAA and PCI DSC 3.1 compliant
-   integrated with stackdriver logging and monitoring
-   autoscale
-   auto upgrade (upgrade nodes)
-   auto repair (repair process for nodes)
-   resource limits (kubernetes specify resource limits of each
    container)
-   stateful applications (storage backed)
-   supports docker image formats
-   fully manager with SRE from google
-   google managed OS
-   private containers (integrates with Google Container Registry)
-   builds with (Google Cloud Build)
-   can be easily transfered to on premise
-   gpu support (easy to run machine learning applications)

## [GKE In-Prem](https://cloud.google.com/gke-on-prem/)

Reliable, efficient, and secured way to run kubernetes clusters
anywhere. Integration using IAM. Similar features to GKE.

## [Cloud Functions](https://cloud.google.com/functions/)

Automatically scales, highly available and fault tolerant. No servers to
provision.

-   file storage events
-   events (pub/sub)
-   http
-   Firebase, and Google Assistant
-   stackdriver logging

Languages: python 3.7 and node.

Access and IAM. VPC access to cloud functions. Can not connect vpc to
cloud functions. IAM controls on the invoke of the function. `--member`
allow you to control which user can invoke the function. IAM check to
make sure that appropriate identity.

Serverless containers are coming soon.

## [Knative](https://cloud.google.com/knative/)

Kubernetes + Serverless addon for GKE.

-   serving: event driven, scale to zero, request driven compute model
-   build: cloud natie source to container orchestration
-   events: universal subscriptin, delivery, and management of events
-   add ons: enable knative addons

## [Sheilded VMs](https://cloud.google.com/shielded-vm/)

Shielded VMs protect against:

-   verify VM identity
-   quickly protect VMs against advanced threats: root kit, etc.
-   protect secrests

Only available for certain images.

## [Containers Security](https://cloud.google.com/containers/security/)

-   patch applied regular on frequent builds and image security reviews
-   containers have less code and a smaller attack surface
-   isolate processes working with recourses (separate storage from
    networking)

# Storage

## [Cloud Storage](https://cloud.google.com/storage/) (Object)

Object storage. Unified S3 object storage api. Great for storage of
blobs and large files (great bandwidth) not necessarily lowest latency.

Single api for accessing:

-   storage
    -   multi-regional (you pay for data transfer between regions)
        -   reduce latency and increse redundancy
    -   regional (higher performance local access and high frequency
        analytics workloads)
-   backup
    -   nearline highly durable storage for data accessed less than
        **once a month**
    -   coldline highly durable storage for data accessed less than
        **once a year**

Object Lifecycle Mangement

-   delete files after older than X days
-   move files to coldline storage after X days

Requester Pays: you can enable requester of resources for pay for
transfer.

You can label a bucket.

You can version all objects.

Encrypt objects with your own encryption keys (locally)

Encrypt objects using Cloud Key Management Storage

Controlling Access:

-   `AllUsers:R` makes an object world readable
-   `allUsers:objectViewer` makes a bucker or group of objects world
    readable
-   Signed URLs give time-limited read or write access to a specific
    Cloud Storage resource
-   programmatically create signed URLs

Redundancy is

## [Persistent Disk](https://cloud.google.com/persistent-disk/) (Block)

Durable and high performance block storage. SSD and HHD available. Can
be attached to any compute engine instance or as google kubernetes
storage volumes. Transparently resized and easy backups. Both can be up
to 64 TB in size. More expensive per GB than storage. No charge for IO.

-   zonal persistent HHD and SSD (efficient reliable block storage)
-   regional peristent disk HHD and SSD. Replicated in two zones
-   local SSD: high performance transient local block-storage
-   cloud storage buckets (mentioned before) affordable object storage

Persistent disk performance is predictable and scales linearly with
provisioned capacity until the limits for an instance's provisioned
vCPUs are reached.

Persistent disks have built-in redundancy.

## [Cloud Storage for Firebase](https://firebase.google.com/products/storage/) (Object)

Free to get started. Uses google cloud storage behind the scenes. Easy
way to provide access to files to users based on authentication. Trigger
functions to process these files. Clients provides sdks for reliable
uploads on spotty connections. Targets mobile.

## [Cloud Filestore](https://cloud.google.com/filestore/) (Filesystem)

Cloud Filestore is a managed file storage service (NAS).

Connects to compute instances and Kubernetes engine instances. Low
latency file operations. Performance equivalent to a typical HHD. Can
get SSD performance for a premium.

Size must be between 1 TB - 64 TB. Price per gigabyte per hour. About 5x
more expensive than object storage. About 2-3X more expensive than Blob
storage.

Cloud filestore exists in the zone that you are using.

Is an NFS fileshare.

## [Drive Enterprise](https://cloud.google.com/drive-enterprise/) (Google Drive)

$8 per active user/month + $0.04 per GB/month and includes Google Docs,
Sheets, and Slides.

# Migration

Covers sending files to google.

Data Transfer (Online Transfer)  
Use your network to move data to Google Cloud storage

-   [draga and
    drop](https://cloud.google.com/storage/docs/cloud-console#_uploadingdata)
-   gsutil
-   json api inject tool (python api for example)

Cloud Storage Transfer Service  
use for cloud transfters like AWS -&gt; GCP

-   Storage Transfer Service allows you to quickly import online data
    into Cloud Storage. You can also set up a repeating schedule for
    transferring data, as well as transfer data within Cloud Storage,
    from one bucket to another.
    -   schedule one time transfer operations or recurring transfer
        operations
    -   deleete existing object in the destination bucket if they dont
        have a corresping object in the source
    -   delete source objects after transfering them
    -   schedule periodic synchronization from source to data (with
        filters)

Transfer Appliance  
install storage locally move data and send to google

-   two rackable applicances capable of 100 TB + 100 TB
-   standalone 500 TB - 1 PB storage to transfer.
-   greater than 20 TB it is worth it.
-   capable of high upload speeds (&gt; 1GB per second)

-   Big Query Data Transfer Service
    -   The BigQuery Data Transfer Service automates data movement from
        Software as a Service (SaaS) applications such as Google Ads and
        Google Ad Manager on a scheduled, managed basis. Your analytics
        team can lay the foundation for a data warehouse without writing
        a single line of code.
    -   data sources :: campaign manager, cloud storage, google ad
        manager, google ads, google play, youtube channel reports,
        youtube content owner reports.

# Databases

## [Cloud SQL](https://cloud.google.com/sql/) (MYSQL POSTGRESQL)

Transactions.

Fully manged MYSQL and Postgresql service. Susstained usage discount.
Data replication between zones in a region.

-   Fully managed MySQL Community Edition databases in the cloud.
-   Second Generation instances support MySQL 5.6 or 5.7, and provide up
    to 416 GB of RAM and 10 TB data storage, with the option to
    automatically increase the storage size as needed.
-   First Generation instances support MySQL 5.5 or 5.6, and provide up
    to 16 GB of RAM and 500 GB data storage.
-   Create and manage instances in the Google Cloud Platform Console.
-   Instances available in US, EU, or Asia.
-   Customer data encrypted on Google’s internal networks and in
    database tables, temporary files, and backups.
-   Support for secure external connections with the Cloud SQL Proxy or
    with the SSL/TLS protocol.
-   Support for private IPbeta (private services access).
-   Data replication between multiple zones with automatic failover.
-   Import and export databases using mysqldump, or import and export
    CSV files.
-   Support for MySQL wire protocol and standard MySQL connectors.
-   Automated and on-demand backups, and point-in-time recovery.
-   Instance cloning.
-   Integration with Stackdriver logging and monitoring.
-   ISO/IEC 27001 compliant.

Some statements and features are not supported.

## [Cloud BigTable](https://cloud.google.com/bigtable/) (NoSQL similar to Cassandra HBASE)

High throughput and consistent. Sub 10 ms latency. Scale to billions of
rows and thousands of columns. can store TB and PB of data. Each row
consists of a key. Large amounts of single keyed data with low latency.
High read and write throughput. Apache HBase API.

Replication among zones. Key value map.

-   key to sort among rows
-   column families for combinations of columns

Performance

-   row keys should be evenly spread among nodes

How to choose a row key:

-   reverse domain names (domain names should be written in reverse)
    com.google for example.
-   string identifiers (do not hash)
-   timestamp in row key
-   row keys can store multiple things - keep in mind that keys are
    sorted (lexicographically)

Cloud bigtable

-   10 MB per cell and 100 MB per row max

## [Cloud Spanner](https://cloud.google.com/spanner/) (SQL)

Globally consistent cloud database.

Fully managed.

Relational Semantics requiring explicit key.

Highly available transactions. Globally replicated.

## [Cloud Datastore](https://cloud.google.com/datastore/) (NoSQL)

SQL like query language. ACID transactions. Fully managed.

Eventually consistent. Not for relational data but storing objects.

-   Atomic transactions. Cloud Datastore can execute a set of operations
    where either all succeed, or none occur.
-   High availability of reads and writes. Cloud Datastore runs in
    Google data centers, which use redundancy to minimize impact from
    points of failure.
-   Massive scalability with high performance. Cloud Datastore uses a
    distributed architecture to automatically manage scaling. Cloud
    Datastore uses a mix of indexes and query constraints so your
    queries scale with the size of your result set, not the size of your
    data set.
-   Flexible storage and querying of data. Cloud Datastore maps
    naturally to object-oriented and scripting languages, and is exposed
    to applications through multiple clients. It also provides a
    SQL-like query language.
-   Balance of strong and eventual consistency. Cloud Datastore ensures
    that entity lookups by key and ancestor queries always receive
    strongly consistent data. All other queries are eventually
    consistent. The consistency models allow your application to deliver
    a great user experience while handling large amounts of data and
    users.
-   Encryption at rest. Cloud Datastore automatically encrypts all data
    before it is written to disk and automatically decrypts the data
    when read by an authorized user. For more information, see
    Server-Side Encryption.
-   Fully managed with no planned downtime. Google handles the
    administration of the Cloud Datastore service so you can focus on
    your application. Your application can still use Cloud Datastore
    when the service receives a planned upgrade.

## [Cloud Memorystore](https://cloud.google.com/memorystore/) (redis)

-   redis as a service
-   high availablility, failover, patching, and monitoring
-   sub millisecond latency and throughput
-   can support up to 300 GB instances with 12 Gbps throughput
-   fully compatible with redis protocol

Ideal for low latency data that must be shared between workers. Failover
is separate zone. Application must be tollerant of failed writes.

## [Cloud Firestore (Datastore like)](https://cloud.google.com/firestore/)

NoSQL database built for global apps. Compatible with Datastore API.
Automatic multi region replication. ACID transactions. Query engine.
Integrated with firebase services.

## [Cloud firebase Realtime Database](https://firebase.google.com/products/realtime-database/)

Real time syncing of JSON data. Can collaborate across devices with
ease.

Could this be used for jupyter notebooks? Probably not due to
restrictions… cant see exactly what text has changed.

# Networking

## Virtual Private Cloud (VPC)

A private space within google cloud platform. A single VPC can span
multiple regions without communicating accress public Internet. Can
allow for single connection points between VPC and on-premise resources.
VPC can be applied at the organization level outside of projects. No
shutdown or downtime when adding IP scape and subnets.

Get private access to google services such as storage big data, etc.
without having to give a public ip.

VPC can have:

-   firewall rules
-   routes (how to send traffic)
-   forwarding rules
-   ip addresses

Ways to connect:

-   vpn (using ipsec)
-   interconnect

## [Google Cloud Load Balancing](https://cloud.google.com/load-balancing/)

Cloud load balances can be divided up as follows:

-   global vs. regional load balancing
-   external vs internal
-   traffic type

Global:

-   http(s) load balancing
-   ssl proxy
-   tcp proxy

Global has a single anycast address, health checks, cookie-based
affinity, autoscaling, and monitoing.

Regional:

-   internal TCP/UDP load balancing
-   network tcp/udp load balancing

Single IP address per region.

## [Google Cloud Armor](https://cloud.google.com/armor/)

Protect against DDoS attacks. Permit deny/allow based on IP.

## [Cloud CDN](https://cloud.google.com/cdn/)

Low-latency, low-cost content delivery using Google global network.
Recently ranked the fastest cdn. 90 cache sites. Always close to users.
Cloud CDN comes with SSL/TLS.

-   anycast (single IP address)
-   http/2 support these push abilities
-   https
-   invalidation :: take down cached content in minutes
-   logging with stackdriver
-   serve content from compute engine and cloud storage buckets. Can mix
    and match.

## [Cloud Interconnect](https://cloud.google.com/interconnect/)

Connect directly to google.

-   interconnect (Direct access with SLA)
    -   dedicated interconnect
    -   partner interconnect
    -   ipsec VPN
-   peering (google's public ips only) no physical connection to google
    -   direct peering
    -   carries peering

## [Cloud DNS](https://cloud.google.com/dns/)

Scalable, reliable, and amanaged DNS syetm. Guarantees 100%
availability. Can create millions of DNS records. Managed through API.

## Network Service Tiers (no studied)

## Network Telemetry

Allows for monitoring on network traffic patterns.

# Management Tools

-   [Stackdriver](https://cloud.google.com/stackdriver/)
    -   monitoring
    -   logging
    -   error reporting with triggers
    -   trace
-   Cloud Console :: web interface for working with google cloud
    recources
-   Cloud shell :: command line management for web browser
-   Cloud mobile app :: available on android and OSX
-   Seperate billing accounts for managing paying for projects
-   Cloud Deployment Manager for managing google cloud infrastructure
-   Cloud apis for all GCP services

# API Platform and Ecosystems

## Apigee API Platform

Many many features around apis. Features: design, secure, deploy,
monitor, and scale apis. Enforce api policies, quota management,
trasformation, autorization , and access control.

-   create api proxies from Open API specifications and deploy in the
    cloud.
-   protect apis. oauth 2.0, saml, tls, and protection from traffic
    spikes
    -   dynamic routing, caching, and rate limiting policies
-   publish apis to a developer portar for developers to be able to
    explore
-   measure performance and usage integrating with stackdriver.

Free trial, then $500 quickstart and larger ones later. Monetization.

Healthcare, Banking, Sense (protect from attacks).

## API Monetization

Tools for creating billing reports for users. Flexible report models
etc. This is through apigee

$300/month to start. Engagement, operational metrics, business metrics.

## [Cloud Endpoints](https://cloud.google.com/endpoints/)

Google Cloud Endpoints allows a shared backend. Cloud endpoints
annotations. Will generate client libraries for the different languages.

Nginx based proxy. Open API specification and provides insight with
stackdriver, monitoring, trace, and logging.

Control who has acess to your API and validate every call with JSON web
tockens and google api keys. Integration with Firebase Authentication
and Auth0.

Less than 1ms per call.

Generate API keys in GCP console and validate on every API call.

## Developer Portal

Have dashboards and places for developers to easily test the API.

# Developer Tools

Cloud SDK  
cli for GCP products

-   `gcloud` manages authentication, local configuration, developer
    workflow, and interactions with cloud platforms apis

-   bq :: big query through the command line

-   kubectl :: management of kubernetes

-   gsutil :: command line access to manage cloud storage buckets and
    objects

-   powershell tools (which I will never use)

## [Container Registry](https://cloud.google.com/container-registry/)

More than a private docker repository.

-   secure private registry
-   automatically build and push images to private registry
-   vulnerability scanning
-   multiple registries
-   fast and highly available
-   prevent deployment of images that are risky and not allowed

Allows:

-   project names images
-   domain named images
-   access control
-   authentication `gcloud docker -a` to use authenticate with gcp
    credentials
-   `mirror.gcr.io` mirrors docker registry
-   notifications when container images change
-   vulnerability monitoring

## [Cloud Build](https://cloud.google.com/cloud-build/)

Cloud Build lets you commit to deploy in containers quickly.

-   stages: build, test, deploy
-   push changes from Github, cloud source repositories, or bitbucket
-   run builds on high cpu vms
-   automate deployment
-   custom workflows
-   privacy
-   integrations with GKE, app engine, cloud functions, and firebase
-   spinnaker supported on all clouds - 120 build minutes per day.

YAML file to specify build.

## [Cloud Source Repositories](https://cloud.google.com/source-repositories/)

-   unlimited private git repositories
-   build in continuous integration (automatic build and testing)
-   fast code search
-   pub/sub notification
-   permissions
-   logging with stackdriver
-   prevent security keys from being commit.

## [Cloud Scheduler](https://cloud.google.com/scheduler/)

CRON job scheduler. Batch and big data jobs, cloud infrastructure
operations. Automate everything retries, manage all automated tasks in
one place.

Serverless scheduling. Cloud scheduler. HTTP or pub/sub tasks up to 1
minutes intervals.

# Data Analytics

## [Big Query](https://cloud.google.com/bigquery/)

Free up to 1 TB of data analyzed each month and 10 GB stored.

-   Data Warehouses
-   Business Intelegence
-   Big Query
-   Big Query ML
    -   Adding labels in SQL query and training model in SQL
    -   linear regresion, classification logistic regressin, roc curve,
        model weight inspection.
    -   feature distribution analysis
    -   integrations with Data Studio, Looker, and Taeblo

Accessible through REST api and client libraries, including command line
tool.

Data is stored in Capacitor columnar data format and offers the standard
database conceps of tables, paritions, columns, and rows.

Data in Big Query Can be loaded in:

-   batch loads
    -   cloud storage
    -   other google services (example google ad manager)
    -   readable data source
    -   streaming inserts
    -   dml (data manipulation language) statements
    -   google big query IO transformation
-   streaming

Saving money on costs:

-   avoid `SELECT *`
-   use summary routines to only show a few rows
-   use `--dry-run` command it will give you price of query
-   no charge for regular loading of data. Streaming does cost money.

## [Cloud Dataflow](https://cloud.google.com/dataflow/)

Fully managed service for transforming and reacting to data.

automated resource managerment  
Cloud Dataflow automates provisioning and management of processing
resources to minimize latency and maximize utilization; no more spinning
up instances by hand or reserving them.

dynamic work rebalancing  
automated and optimized work partitioning dynamic lagging work.

-   reliable & consistent exactly-once processing
-   horizontal auto scaling
-   apache beam sdk

Realtime processing of incoming data.

-   cleaning up data
-   triggering events
-   writing data to destinations SQL, bigquery, etc.

Regional Endpoints

## [Cloud Dataproc](https://cloud.google.com/dataproc/)

Managed apache hadoop and apache spark instances.

Scalable and allow for preemptible instances.

ETL pipeline (Extract Transform Load)

## [Cloud Datalab](https://cloud.google.com/datalab/)

Jupyter notebooks + magiks that working google cloud.

Big query integration

Gcloud console commands

## [Cloud Pub/Sub](https://cloud.google.com/pubsub/)

simple reliable, scalable foundation for analytics and event driven
computing systems.

Can trigger events such as Cloud Dataflow.

Features:

-   at least once delivery
-   exactly once processing
-   no provisioning
-   integration with cloud storage, gmail, cloud functions etc.
-   open apis and client libraries in many languages
-   globally trigger events
-   pub/sub is hipaa compliant.

Details:

message  
the data that moves through the serivce

topic  
a named entity that represetnds a feed of messages

subscription  
an entity interested in receiving mesages ona particular topic

publisher  
create messages and sends (published) them to a specific topic

subscriber (consumer)  
recieves messages on a specified subscription

Performance(scalability):

## [Cloud Composer](https://cloud.google.com/composer/)

Managed Apache airflow. (differences)

-   client tooling and integrated experience with google cloud
-   security IAM and audit logging with google cloud
-   stackdriver intergration
-   streamlined airflow runtime and environment configuration such as
    plugin support
-   simplified DAG (workflow) maangement
-   python pypi package management

Core support for:

-   dataflow
-   bigquery
-   storage operators
-   spanner
-   sql
-   support in many other clouds
-   workflow orchestration solution

DAGS every workflow is defined as a DAG. Series of tasks to be done.

Task a set task within a workflow that you want to do.

Operators.

Builtin support for services outside GCP: http, sftp, bash , python,
AWS, Azure, Databricks, JIRA, Qubole, Slack, Hive, Mongo, MySQL, Oracle,
Vertica.

Kubernetes PodOperator.

Integrates with `gcloud composer`. Cloud SQL is used to store the
Airflow metadata. App Engine for serving the web service. Cloud storage
is used for storing python plugins and dags etc. All running inside of
GKE. Stackdriver is used for collecting all logs.

## [Genomics](https://cloud.google.com/genomics/)

Process in parallel.

-   fully integrated with GCP products
-   secured with hippa compliant
-   real time processing

## [Google analytics 360 Studio](https://marketingplatform.google.com/about/enterprise/#?modal_active=none)

Enterprise analytics (not going to dive deaply into).

-   understand customer decision better
-   used google adertising system
-   adwords
-   doubleclick
-   allows easy collaboration
-   tag manager
    -   manage analytics tags
-   optimize
    -   personalized messages to audience (text change to website
        measure best variant)
-   google audience
    -   recognize who your most valuable audiences are
-   attriubtion
    -   shows how your messages are converged
-   data studio
    -   data import and data visualization from big query
    -   custom reports
-   surveys
    -   accurate surveys to send out
    -   brand awareness

## [Google Data Studio](https://marketingplatform.google.com/about/data-studio/)

Unite data in one place: spreadsheets, analytics, google ads, big query
all combined Explore the data

-   connectors: many many services including community ones
-   transformation
-   you can cache queries from big query.
    -   big query can cache results temporarily or using a new table

## [Firebase Performance Monitoring](https://firebase.google.com/products/performance/)

Monitoring loading experience for users.

-   wide variety of locations
-   wide variaty of
-   variaty, location, device, etc.

SDK in app you get performance metrics of your app as seen from the
users.

# AI and Machine Learning

## [AI Hub](https://cloud.google.com/ai-hub/)

A collection of end-to-end AI pipelines and out of the box algorithms
for solving specific machine learning problems.

Prebuilt solutions. Very much a pre alpha product.

## [Cloud AutoML](https://cloud.google.com/automl/)

Makes it approachable even if you have minimal experience.

Products:

-   natrual language
-   translation
-   vision

Use case is that you would like to specially train your model to detect
features that are more specific than the ones google provides.

Limited experience to train and make predictions. Full rest api and
scalably train for labeling features.

## [Cloud TPU](https://cloud.google.com/tpu/)

Much faster for machine learning computations and numerical algorithms.

## [Cloud Machine Learning Engine](https://cloud.google.com/ml-engine/)

Focus on models not operations.

-   multiple frameworks built in
    -   scikit-klearn, xgboost, keas, tensorflow
-   automate hyperparameter search
-   automate resource provisioning
-   train with small models for development and easily scale up
-   send computations to the cloud
-   cloud ml engine works with cloud dataflow

Manage model version.

gcloud commands work with it.

## [Cloud Talent Solution](https://cloud.google.com/solutions/talent-solution/)

Machine learning to solve the job search

## [Dialog Flow](https://cloud.google.com/dialogflow-enterprise/)

-   conversational interfaces
    -   chatbots for example
    -   text to speech
    -   20+ languages supported
    -   user sentiment analysis
    -   spelling correction

## [Cloud Natrual Language](https://cloud.google.com/natural-language/)

Google Cloud Natural Language reveals the structure and meaning of text
both through powerful pretrained machine learning models in an easy to
use REST API and through custom models that are easy to build with
AutoML Natural LanguageBeta. Learn more about Cloud AutoML.

You can use Cloud Natural Language to extract information about people,
places, events, and much more mentioned in text documents, news
articles, or blog posts. You can use it to understand sentiment about
your product on social media or parse intent from customer conversations
happening in a call center or a messaging app. You can analyze text
uploaded in your request or integrate with your document storage on
Google Cloud Storage.

## [Cloud Speech to Text](https://cloud.google.com/speech-to-text/)

Google Cloud Speech-to-Text enables developers to convert audio to text
by applying powerful neural network models in an easy-to-use API. The
API recognizes 120 languages and variants to support your global user
base. You can enable voice command-and-control, transcribe audio from
call centers, and more. It can process real-time streaming or
prerecorded audio, using Google’s machine learning technology.

## [Cloud Text-to-Speech](https://cloud.google.com/text-to-speech/)

Google Cloud Text-to-Speech enables developers to synthesize
natural-sounding speech with 30 voices, available in multiple languages
and variants. It applies DeepMind’s groundbreaking research in WaveNet
and Google’s powerful neural networks to deliver high fidelity audio.
With this easy-to-use API, you can create lifelike interactions with
your users, across many applications and devices.

## [Cloud Translation](https://cloud.google.com/translate/)

Cloud Translation offers both an API that uses pretrained models and the
ability to build custom models specific to your needs, using AutoML
Translation.

The Translation API provides a simple programmatic interface for
translating an arbitrary string into any supported language using
state-of-the-art Neural Machine Translation. It is highly responsive, so
websites and applications can integrate with Translation API for fast,
dynamic translation of source text from the source language to a target
language (such as French to English). Language detection is also
available in cases where the source language is unknown. The underlying
technology is updated constantly to include improvements from Google
research teams, which results in better translations and new languages
and language pairs.

## [Cloud Vision](https://cloud.google.com/vision/)

Cloud Vision offers both pretrained models via an API and the ability to
build custom models using AutoML Vision to provide flexibility depending
on your use case.

Cloud Vision API enables developers to understand the content of an
image by encapsulating powerful machine learning models in an
easy-to-use REST API. It quickly classifies images into thousands of
categories (such as, “sailboat”), detects individual objects and faces
within images, and reads printed words contained within images. You can
build metadata on your image catalog, moderate offensive content, or
enable new marketing scenarios through image sentiment analysis.

## [Cloud Inference API](https://cloud.google.com/inference/docs/)

Quickly run large-scale correlations over typed time-series datasets.

Calculate correlations between data that you are getting from sensors
etc. For example using big table.

## [Firebase Predictions](https://firebase.google.com/products/predictions/)

Group users based on predictive behavior.

## [Cloud Deep Learning VM Image](https://cloud.google.com/deep-learning-vm/)

Preconfigured images for deep learning application.

Have all the interesting frameworks preinstalled.

# Security

Learn if necessary.

## Cloud IAM

Identity. Access is granted to `members`. Memebers can be of several
types:

-   google account (any google account not necissarily `@gmail.com`.
-   service account (created for individual applications)
-   google group `group@domain.com` (email address that contains several
    google users) cannot establish identity
-   gsuite domain `domain.com` (cannot establish identity)
-   allAuthenticatedUsers (any signed in google account)
-   allUsers anyone on the web

Resource:

-   you can grant access to `<service>.<resource>` resources

Permissions:

-   you can grant access based on `<service>.<resource>.<verb>`.

Roles are collections of permissions. Three kinds of roles in Cloud IAM.

-   primitive roles: Owner, Editor, Viewer
-   predefined roles: finer access than primitive roles.
    `roles/pubsub.publisher` provides access to only publish messages to
    a cloud pub/sub topic.
-   custom roles: custom roles specific to the organization.

Cloud IAM policies bind `member` -&gt; `roles`.

Resource Hierarchy

You can set a Cloud IAM policy at any level in the resource hierarchy:
the organization level, the folder level, the project level, or the
resource level. Resources inherit the policies of the parent resource.
If you set a policy at the organization level, it is automatically
inherited by all its children projects, and if you set a policy at the
project level, it's inherited by all its child resources. The effective
policy for a resource is the union of the policy set at that resource
and the policy inherited from higher up in the hierarchy.

Best Practices:

-   Mirror your IAM policy hierarchy structure to your organization
    structure.
-   Use the security principle of least privilege to grant IAM roles,
    that is, only give the least amount of access necessary to your
    resources.
-   grant roles to groups when possible
-   grant roles to smallest scope needed
-   billing roles for administration looking over
-   prefer predefined roles (not primitive)
-   owner allows modifying IAM policy so grant carefully
-   cloud audit logs to monitor changes to IAM policy

Service Accounts:

-   belong to an application or virtual machine instead of user
-   Each service account is associated with a key pair, which is managed
    by Google Cloud Platform (GCP). It is used for service-to-service
    authentication within GCP. These keys are rotated automatically by
    Google, and are used for signing for a maximum of two weeks.
-   You should only grant the service account the minimum set of
    permissions required to achieve their goal.
-   Compute Engine instances need to run as service accounts
-   establish naming convention for service account
