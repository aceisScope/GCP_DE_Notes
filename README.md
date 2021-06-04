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

**Unstructed** object storage. Unified S3 object storage api. Great for storage of
blobs and large files (great bandwidth) not necessarily lowest latency.

Storage Classes:

-   Standard
-   Nearline: 30-days minimum
-   Coldline: 90-days minimum
-   Archive: 365-days minimum

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

Advanced Features:

-   Requester Pays: you can enable requester of resources for pay for transfer.
-   Parallel uploads of composite objects
-   Integrity checking: MD5 hash
-   Transcoding: gzip

You can label a bucket.

You can version all objects. Overwrites are atomic.

Encrypt objects with your own encryption keys (locally)

Encrypt objects using Cloud Key Management Storage

Controlling Access:

-   `AllUsers:R` makes an object world readable
-   `allUsers:objectViewer` makes a bucker or group of objects world
    readable
-   IAM for bulk access to buckets
-   ACL for granular access to buckets
-   Signed policy document for buckets
-   Signed URLs give time-limited read or write access to a specific Cloud Storage resource



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

## Data Transfer (Online Transfer)  
Use your network to move data to Google Cloud storage

-   [draga and
    drop](https://cloud.google.com/storage/docs/cloud-console#_uploadingdata)
-   gsutil
-   json api inject tool (python api for example)

## Cloud Storage Transfer Service  
use for cloud transfters like: AWS S3, HTTP sources and other GCS buckets 

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

## Transfer Appliance  
install storage locally move data and send to google

-   two rackable applicances capable of 100 TB + 100 TB
-   standalone 500 TB - 1 PB storage to transfer.
-   greater than 20 TB it is worth it.
-   capable of high upload speeds (&gt; 1GB per second)

## Big Query Data Transfer Service
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
-   MySQL 5.6 or 5.7, PostgreSQL 9.6 or 11 and SQL Server in beta
-   Customer data encrypted on Google’s internal networks and indatabase tables, temporary files, and backups.
-   Support for secure external connections with the Cloud SQL Proxy or with the SSL/TLS protocol, or private IPbeta (private services access).
-   MySQL: 
    -   HA: Data replication between multiple zones with automatic failover via failover replicas.
    -   Import and export databases using mysqldump, CSV files or external replica promotion (requires binary log retention and GTID consistency).
    -   Support for MySQL wire protocol and standard MySQL connectors.
    -   Automated and on-demand backups, and point-in-time recovery (requires **binary logging**).
-   Postgres:
    -  HA: primary instance and standby instance in multiple zones sharing the same persistent disk
    -  Automated backups, but doesn't support point-in-time recovery
    -  Extensions   
-   Instance cloning.
-   Integration with Stackdriver logging and monitoring.
-   ISO/IEC 27001 compliant.

Some statements and features are not supported.

## [Cloud BigTable](https://cloud.google.com/bigtable/) (NoSQL similar to Cassandra HBASE)

Managed wide-column NoSQL database. High throughput. Low latency. Scalibility. High availability. Apache HBase API.

### Data Model
-   Row key: the only index
-   Column families: logical combinations of columns
-   Column qualifier: the name of the column
-   3-D table: every cell is written with a timestamp. new values can be added without overwriting the old value. a value of a cell is an array of bytes.
-   Tablets: Block of continous rows are sharded into *tablets*. Within a tablet, rows are sorted by row key.
-   Sparse table
-   A cell is max 10Mb, a row is max 100 Mb.
-   Timestamp and garbage collection: each cell has multiple versions. older versions can be garbage collected according to expiry policy based on age or numbers of versions.

### Schema Design

- Query: uses a row key or a row key prefix
- Designing row keys
    -   reverse domain names (domain names should be written in reverse) com.google for example.
    -   string identifiers (do not hash)
    -   timestamp in row key (only as part of a bigger row key design)
    -   row keys can store multiple things - keep in mind that keys are sorted (lexicographically)
- Designing for performance:
    -   Lexicographic sorting
    -   Store related entities in adjecent rows
    -   Distribute reads and writes evenly
    -   Balanced access patterns enable linear scaling of performance
- Designing for time series data:
    - Use tall and narrow tables  
    - Use rows instead of versioned cells
    - Logically separate tables
- Avoid hotspots:
    - Field promotion: taking data that is already known and moving it to the row key.
    - Salting
    - Key visualizer

### Architecture

-   Instance: a logical container for bigtable cluster that shares a same configuration. defined by instance type, storage type and app profiles. Up to 4 clusters. Maximum 1000 tables.
    -   Instance types:
        - Development: single node cluster. no replication or SLA. can be upgraded to production.
        - Production: 1+ clusters, 3+ nodes per cluster.
    -   Storage types: 
        -   SSD
        -   HDD: storing at least 10TB of infrequently-accessed data with no latency sensitivity
    -   Application profiles: Custome application-specific settings for handling income connections. Single or multi-cluster routing. **Single-row transactions (atomic update to single row) support requies single cluster routing for strong consistency**.    
-   Cluster: inside instance, contains nodes. To achieve multi-zone redundancy is to run additional clusters in different zones in the same instance. Up to 30 clusters per project.
-   Data storage: tablets in Coogle Colossus

### Monitoring

- CPU overload: for single-cluster instance, average CPU load should be under 70%, hottest node not going over 90%. For two clusters, 35% and 45% respectively.
- Storage utilization: below 70% per node

### Performance

- Replications improves read throughput but doesn't affect write throughput
- Use batch writes for bulk data with rows that are close together lexicographically

## [Cloud Spanner](https://cloud.google.com/spanner/) (SQL)

Globally consistent cloud database. Horizontally scalable.

Fully managed.

Relational Semantics requiring explicit key.

Highly available transactions. Globally replicated.

## [Cloud Firetore](https://cloud.google.com/firestore/) (NoSQL, like MongoDB)

NoSQL document store. Realtime DB with mobile SDKs. SQL like query language. ACID transactions. Fully managed.

Eventually consistent. Not for relational data but storing objects.

-   Data types: 
    -   string, integer, boolean, float, null
    -   bytes, date and time, geographical point
    -   array and map
    -   *reference*
-   Automatically create single-field indexes
-   Atomic transactions. Cloud Firetore can execute a set of operations
    where either all succeed, or none occur.
-   High availability of reads and writes. Cloud Firetore runs in
    Google data centers, which use redundancy to minimize impact from
    points of failure.
-   Massive scalability with high performance. Cloud Firetore uses a
    distributed architecture to automatically manage scaling. Cloud
    Datastore uses a mix of indexes and query constraints so your
    queries scale with the size of your result set, not the size of your
    data set.
-   Flexible storage and querying of data. Cloud Firetore maps
    naturally to object-oriented and scripting languages, and is exposed
    to applications through multiple clients. It also provides a
    SQL-like query language.
-   Balance of strong and eventual consistency. Cloud Firetore ensures
    that entity lookups by key and ancestor queries always receive
    strongly consistent data. All other queries are eventually
    consistent. The consistency models allow your application to deliver
    a great user experience while handling large amounts of data and
    users.
-   Encryption at rest. Cloud Firetore automatically encrypts all data
    before it is written to disk and automatically decrypts the data
    when read by an authorized user. For more information, see
    Server-Side Encryption.
-   Fully managed with no planned downtime. Google handles the
    administration of the Cloud Firetore service so you can focus on
    your application. Your application can still use Cloud Datastore
    when the service receives a planned upgrade.
-   Realtime update: `on_snapshot` function listens for updates and callbacks can act on updates

## [Cloud Memorystore](https://cloud.google.com/memorystore/) (redis)

-   redis as a service, no need to provision VMs
-   high availablility, failover, patching, and monitoring
-   sub millisecond latency and throughput
-   can support up to 300 GB instances with 12 Gbps throughput
-   fully compatible with redis protocol

Ideal for low latency data that must be shared between workers. Failover
is separate zone. Application must be tollerant of failed writes.

User cases include session cache, message queue and pub/sub.


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

Free up to 1 TB of data analyzed each month and 10 GB stored. High availability, support SQL and federated data.

### Features
-   Data Warehouses
-   Business Intelegence
-   Big Query ML
    -   Adding labels in SQL query and training model in SQL
    -   linear regresion, classification logistic regressin, k-means clustering, roc curve, model weight inspection.
    -   feature distribution analysis
    -   integrations with Data Studio, Looker, and Tableu

### Data Ingestion

Data in Big Query Can be loaded in:

-   batch loads
    -   cloud storage
    -   other google services (example google ad manager)
    -   readable data source
    -   streaming inserts
    -   dml (data manipulation language) statements
    -   google big query IO transformation
-   streaming

### Usage

- Jobs: Load, export, query (priorities: interactive and batch), copy
- Table storage: data is stored in Capacitor columnar data format and offers the standard database conceps of tables, paritions, columns, and rows.
    - Capacitor columnar: Proprietary columnar data storage that supports semi-structured data (nested and repeated fields). Each value stored together with a repetition level and a definition level
- Denormalisation: nested and repeated columns -> **RECORD (STRUCT)** data type
- View: a virtual table defined by an SQL query. each time a view is accessed the query is executed.
    - Control access to data
    - Reduce query complexity
    - Constructing logical tables
    - Ability to create authorised views (1000 per dataset) to share data across projects
- Partitioning: can reduce cost and improve query performance
    - Ingestion time based partitioning: partitioned by load or arrival date, **_PARTITIONTIME**
    - partitioned tables: partition based on TIMESTAMP or DATE column
- Clustering: data with a particular cluster key is stored together. to further reduce scan of unnecessary data.
- Slots: unit of computational capacity required to execute SQL query

### Best Practices

#### Controlling Cost

-   avoid `SELECT *`
-   use preview options to sample data
-   use `--dry-run` command it will give you price of query
-   user `LIMIT` doesn't affect cost
-   partition by date
-   materialize query results in stages
-   use streaming inserts with caution

#### Performance

- Input data and data sources
    - Prune partitioned queries
    - Denormalise data whenever possible
    - use external data sources apporiately
    - avoid excessive wildcard tables
- Query computation
    - avoid repeated transforming data via SQL queries
    - avoid JavaScripted user-defined functions
    - order query operations to maximize performance
    - optimize JOIN patterns
- SQL anti-patterns

### Security

- Cloud Data Loss Prevention (DLP) to protect sensitive data
- Cloud Key Management Service

## [Cloud Dataflow](https://cloud.google.com/dataflow/)

Fully managed service for transforming and reacting to data, based on Apache Beam. Cloud Dataflow automates provisioning and management of processing resources to minimize latency and maximize utilization; no more spinning up instances by hand or reserving them.

Driver program defines pipeline, is submitted to a Runner for processing. 

### Concepts
-   PCollection: represents a potentially distributed, multi-element dataset that acts as the pipeline's data. It's immutable.
-   Transform:  represents a processing operation that transforms data. A transform takes one or more PCollections as input, performs an operation that you specify on each element in that collection, and produces one or more PCollections as output. E.g. ParDo, GroupBYKey, CoGroupByKey, Combine, Flatten, Partition.
-   ParDo: the core parallel processing operation in the Apache Beam SDKs, invoking a user-specified function on each of the elements of the input PCollection. ParDo collects the zero or more output elements into an output PCollection. The ParDo transform processes elements independently and possibly in parallel.
-   Aggregation: Aggregation is the process of computing some value from multiple input elements. The primary computational pattern for aggregation in Apache Beam is to group all elements with a common key and window. Then, it combines each group of elements using an associative and commutative operation.
-   Event Time: data event occurs, determined by the timestamp on the data element itself. This *contrasts with the time the actual data element gets processed at any stage in the pipeline*.
-   Windowing: enables grouping operations over unbounded collections by dividing the collection into windows of finite collections according to the timestamps of the individual elements
    -   Fixed time window: represents a constant, non-overlapping time interval
    -   Sliding: can overlap. an element can belong to multiple windows. useful for taking running averages of data.
    -   Per Session: created in a stream when there is an interruption in the flow of the events which exceeds a certain time period. applied on a per-key basis. useful for irregular distributed data.
    -   Single global
-   Watermarks: is the system's notion of when all data in a certain window can be expected to have arrived in the pipeline
-   Triggers: determine when to emit aggregated results as data arrives. For bounded data, results are emitted after all of the input has been processed. For unbounded data, results are emitted when the watermark passes the end of the window. E.g. event time trigger, processing time trigger, data-driven trigger, composite trigger

### Access control

-  Cloud Dataflow service account: automatically created when project is created. used by dataflow service itself. manipulated job resources. assumes **cloud dataflow agent role**. r/w access to project resources.
-  Controller service account: used by the worker (compute engine) instances created by the dataflow service. use for metadata operations. it's possible to create user-managed controller service account.
-  Security mechanisms: Submission of the pipeline, eveluation of the pipeline

### Using Cloud Dataflow

-   Regional Endpoints: A Dataflow regional endpoint stores and handles metadata about your Dataflow job and deploys and controls your Dataflow workers.
-   Customer-managed encryption key: encrypt dataflow data at rest
-   Flexible Resource sharing (FlexRS) for batched pipleline to reduce cost
    -   Advanced scheduling
    -   Cloud Dataflow Shuffle service
    -   Preemptible VMs
-   Migrating MapReduce to dataflow
-   With Cloud Pub/Sub Seek to replay

## [Cloud Dataproc](https://cloud.google.com/dataproc/)

Managed apache hadoop and apache spark instances. ETL pipeline (Extract Transform Load). Nodes are preconfigured VMs. One master and multiple worker nodes (except single-node cluster).

Support native connectors: Cloud Storage, BigQuery and Cloud BigTable

Autoscaling is not recommended with/for HDFS, YARN Node Labels, Spark Structured Streaming or Idle Clusters

### Cluster Types
- Single-node: master and workers are on the same VM
- Standard: A master VM (YARN resource manager and HDFS name node) and worker VMs (YARN node manager and HDFS data node). can also add preemptible workers, but these workers don't store HDFS data.
- High-availability: Three master VMs and multiple workder VMs (autoscaling doesn't work with HA type)


## [Cloud Datalab](https://cloud.google.com/datalab/)

Jupyter notebooks + magiks that working google cloud.

Big query integration

Gcloud console commands

## [Cloud Pub/Sub](https://cloud.google.com/pubsub/)

simple reliable, scalable foundation for analytics and event driven
computing systems. serverless and fully-managed.

Can trigger events such as Cloud Dataflow.

### Features:

-   **at least once delivery**: each message is delivered at least once for every subscription. Undelivered message will be deleted after the message retention duration, default is 7 days (10 mins - 7 days).
-   exactly once processing
-   no provisioning
-   integration with cloud storage, gmail, cloud functions etc.
-   open apis and client libraries in many languages
-   globally trigger events
-   pub/sub is hipaa compliant.
-   Seeking: to alter the state of acknowledgement of messages in bulk. For example, you can seek to a timestamp in the past, and all messages received after that time will be marked as unacknowledged
-   Snapshot: are used in seeking as an alternative to a timestamp. You need to create a snapshot at a point in time, and that snapshot will retain all messages that were unacknowledged at the point of the snapshot's creation, accessing telemetry or metrics

### Details:

message  
the data that moves through the serivce. message must be acknowledged on receiving. PULL is the default method. If PUSH is used, must use HTTPS. 

topic  
a named entity that represetnds a feed of messages

subscription  
an entity interested in receiving mesages on a particular topic. Expires after 31 days of inactivity. 

publisher  
create messages and sends (published) them to a specific topic. A messae is base64 encoded, 10MB or less.

subscriber (consumer)  
recieves messages on a specified subscription. 

### Monitoring

- `pubsub.googleapis.com/topic/byte_cost`: Total utilization in bytes
- `pubsub.googleapis.com/subscription/byte_cost`: Subscription utilization in bytes
- `subscription/num_undelivered_messages`: Undelivered messages belonging to a subscription
- `subscription/oldest_unacked_message_age`: The oldest message yet to be retrieved by a subscription
- `subscription/num_outstanding_messages`: Messagings pending delivery to a push subscription

### Access Control

- Use service accounts for authorization
- Grant per-topic or per-subscription permissions
- Grant limited access to publish or consume messages

## [Cloud Composer](https://cloud.google.com/composer/)

Task orchestration system. Managed Apache airflow. Every workflow is defined as a DAG. 

### Architecture

- A Cloud Composer environment is a wrapper around Apache Airflow. It includes:
    - GKE Cluster: The Airflow scheduler, workers, and Redis Queue run as GKE workloads on a single cluster, and are responsible for processing and executing DAGs. 
    - Web server: The web server runs the Apache Airflow web interface
    - Cloud Storage bucket: Cloud Composer associates a Cloud Storage bucket with the environment
- Cloud Composer environments can be created in three configurations: Public IP, Private IP, and Private IP with Domain restricted sharing (DRS)
    - In a Public IP configuration, environment resources are distributed between the customer and tenant projects. The tenant project hosts a Cloud SQL instance to run the Airflow database, and a Google App Engine Flex VM to run the Airflow web server. The Airflow scheduler, workers, and webserver connect to the Airflow database using Cloud SQL proxy processes
    - Environment variables can be passed by the environment to the scheduler and workers etc.
- An Airflow DAG is defined in a Python file and is composed of the following components: A DAG definition, operators, and operator relationships.
- Airflow Connections: default to BigQuery, Datastore, Cloud Storage, Generic


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

- Intents: training phrases -> intents classification -> extracted parameters
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
AutoML Natural Language. 

- Sentiment Analysis
    - Score: indicates the overall emotionality, between -1.0 (negative) to 1.0 (positive)
    - Magnitutde: how much emotional content. from 0 to infinity. not normalised.
- Entity Analysis: inspects the given text for known entities (proper nouns such as public figures, landmarks, etc.), and returns information about those entities, including metadata containing a knowledge graph MID and a wikipedia url if identified, salience (importance), etc
- Entity sentiment analysis: combines Entity and Sentiment Analysis. tries to determine the sentiment expressed towards each of the identified entities.
- Content classification

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


## [Cloud Vision](https://cloud.google.com/vision/)

Cloud Vision offers both pretrained models via an API and the ability to
build custom models using AutoML Vision to provide flexibility depending
on your use case.
- OCR: `TEXT_DETECTION` and `DOCUMENT_TEXT_DETECTION`
- Cropping hints
- Face detection: faces, emotional states, hatware etc
- Image property detection
- Label detection: objects, locations, etc
- Landmark detection
- Logo detection
- Explicit content detection
- Web entity and page detection

Google knowledge graph search API: 
- Getting a ranked list of the most notable entities that match certain criteria.
- Predictively completing entities in a search box.
- Annotating/organizing content using the Knowledge Graph entities.

## [Cloud Video AI](https://cloud.google.com/video-intelligence)

Time duration based powerful content discovery with videos.

- Detect labels
- Shot change detection
- Detect explicit content
- Transscribe speech
- Track objects
- Detect text

# Security

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
