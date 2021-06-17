
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

## Data Transfer Service
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

Up to 30TB.

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
        -   SSD: reads up to up to 10,000 rps.
        -   HDD: up to 500 rps. storing at least 10TB of infrequently-accessed data with no latency sensitivity
        -   one can't change the disk type on an existing Bigtable instance
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

Cloud Spanner automatically creates an index for each table's primary key. You can also create _secondary indexes_ for other columns

## [Cloud Firetore](https://cloud.google.com/firestore/) (NoSQL, like MongoDB)

NoSQL document store. Realtime DB with mobile SDKs. SQL like query language. ACID transactions. Fully managed.

Eventually consistent. Not for relational data but storing objects.

-   Data types: 
    -   string, integer, boolean, float, null
    -   bytes, date and time, geographical point
    -   array and map
    -   *reference*
-   Automatically create single-field indexes
-   Composite indexes: A composite index stores a sorted mapping of all the documents in a collection, based on an ordered list of fields to index.
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


# Data Analytics

## [Cloud BigQuery](https://cloud.google.com/bigquery/)

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
- Time travel: you can query a snapshot of a table from any point in time within the previous seven days by using a `FOR SYSTEM_TIME AS OF` clause
- [Large-scale mutations](https://cloud.google.com/blog/products/bigquery/performing-large-scale-mutations-in-bigquery) 
- [Loading data](https://cloud.google.com/bigquery/docs/loading-data#loading_encoded_data):
    - Batch or stream
    - Format:  Avro, ORC, Parquet, CSV & JSON (provide an explicit schema, or you can use schema auto-detection)
    - Encoding: BigQuery supports UTF-8 encoding for both nested or repeated and flat data. BigQuery supports ISO-8859-1 encoding for flat data only for CSV files.

### Pricing

- BigQuery charges for storage, queries, and streaming inserts
- On-demand pricing. With this pricing model, you are charged for the number of bytes processed by each query. The first 1 TB of query data processed per month is free
- Flat-rate pricing. With this pricing model, you purchase slots, which are virtual CPUs. When you buy slots, you are buying dedicated processing capacity that you can use to run queries

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
    - Denormalise data whenever possible
    - use external data sources apporiately
    - avoid excessive wildcard tables
- Query computation
    - avoid repeated transforming data via SQL queries
    - avoid JavaScripted user-defined functions
    - Use approximate aggregation functions: e.g. instead of using `COUNT(DISTINCT)`, use `APPROX_COUNT_DISTINCT()`
    - order query operations to maximize performance: Use ORDER BY only in the outermost query or within window clauses (analytic functions). Push complex operations to the end of the query
    - optimize JOIN patterns: large table as the left side of the JOIN and a small one on the right side of the JOIN
    - Prune partitioned queries: When querying a partitioned table, use the `_PARTITIONTIME` pseudo column to filter the partitions.
- SQL anti-patterns
- Using cached query results: BigQuery writes all query results to a table. The table is either explicitly identified by the user (a destination table), or it is a temporary, cached results table. Caches are valid for 24hrs. 

#### Optimizing storage

Best practice: Keep your data in BigQuery. Rather than exporting your older data to another storage option (such as Cloud Storage), take advantage of BigQuery's long-term storage pricing. If you have a table that is not edited for 90 consecutive days, the price of storage for that table automatically drops to the same cost as Cloud Storage Nearline.

### Security
- Dataset level
- Table ACL: tables and views
- Restrict access to columns 
- Cloud Data Loss Prevention (DLP) to protect sensitive data
- Cloud Key Management Service
- Basic roles for projects: Viewer, Editor, Owner; Basic roles for datasets: READER, WRITER, OWNER

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
-   Triggers: determine when to emit aggregated results as data arrives. For bounded data, results are emitted after all of the input has been processed. For unbounded data, results are emitted when the watermark passes the end of the window. 
    -   event time trigger
    -   processing time trigger
    -   number of data elements in a collection

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
-   With Cloud Pub/Sub Seek to replay
-   `Cancel` will shut down the pipeline without allowing buffered jobs to complete. `Drain` will stop new data from flowing in but will leave the processing pipeline running to process buffered data
-   Pushing data to multiple storage locations: use Bigtable and BigQueryIO transforms on PCollection

## [Cloud Dataproc](https://cloud.google.com/dataproc/)

Managed apache hadoop and apache spark instances. ETL pipeline (Extract Transform Load). Nodes are preconfigured VMs. One master and multiple worker nodes (except single-node cluster).

Support native connectors: Cloud Storage, BigQuery and Cloud BigTable

Autoscaling is not recommended with/for **HDFS, YARN Node Labels, Spark Structured Streaming or Idle Clusters**

### Cluster Types
- Single-node: master and workers are on the same VM
- Standard: A master VM (YARN resource manager and HDFS name node) and worker VMs (YARN node manager and HDFS data node). can also add preemptible workers, but these workers don't store HDFS data.
- High-availability: Three master VMs and multiple workder VMs (autoscaling doesn't work with HA type)

### Migrating Hadoop Jobs from On-Premises to Datapro
[https://cloud.google.com/architecture/hadoop/hadoop-gcp-migration-jobs](https://cloud.google.com/architecture/hadoop/hadoop-gcp-migration-jobs)


## [Cloud Datalab](https://cloud.google.com/datalab/)

Jupyter notebooks + magiks that working google cloud.

Cloud Datalab instances are single-user environments, therefore each member of your team needs their own instance. Synchronize changes to the shared Cloud Source Repository enables team sharing.

## [Cloud Pub/Sub](https://cloud.google.com/pubsub/)

simple reliable, scalable foundation for analytics and event driven
computing systems. serverless and fully-managed.

Can trigger events such as Cloud Dataflow.

### Features:

-   **at least once delivery**: each message is delivered at least once for every subscription. Undelivered message will be deleted after the message retention duration, default is 7 days (10 mins - 7 days).
-   exactly once processing
-   no provisioning
-   integration with cloud storage, gmail, cloud functions etc.
-   globally trigger events
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


## [Google Data Studio](https://marketingplatform.google.com/about/data-studio/)

Unite data in one place: spreadsheets, analytics, google ads, big query all combined Explore the data

-   connectors: many many services including community ones. Different data connectors in Data Studio have different refresh rates. In between data refreshes, Data Studio will use its cache to present data. The cache can be **manually refreshed** however.
-   transformation
-   you can cache queries from big query.
    -   big query can cache results temporarily or using a new table

## [Google Dataprep](https://cloud.google.com/dataprep)

An intelligent cloud data service to visually explore, clean, and prepare data for analysis and machine learning

# AI and Machine Learning

## [Machine Learning Basics](https://developers.google.com/machine-learning/glossary/#l)

### Concepts
- feature: An input variable used in making predictions.
- hyperparameter: The "knobs" that you tweak during successive runs of training a model. For example, batch size, training epochs, number of hidden layers, regularizaiton types, regularization rate and learning rate
- label: In supervised learning, the "answer" or "result" portion of an example
- overfitting: Creating a model that matches the training data so closely that the model fails to make correct predictions on new data. How to avoid: Early stopping, Train with more data, Data augmentation, Feature selection, Regularization, Ensemble methods, Cross-validation (k-folds), Dropout layers (neural networks)
    - L1 regularization: penalizes weights in proportion to the sum of the absolute values of the weights. helps drive the weights of irrelevant or barely relevant features to exactly 0, which removes those features from the model
    - L2 regularization: penalizes weights in proportion to the sum of the squares of the weights. helps drive outlier weights (those with high positive or low negative values) closer to 0 but not quite to 0
- Feature engineering: 
    - Missing data: Ignore, Remove, Impute
    - Outliers and feature clipping
    - One-hot encoding
    - Linear scaling
    - Z-score
    - Log scaling
    - Bucketing

### Types:
- supervised machine learning: Training a model from input data and its corresponding labels.
- unsupervised machine learning: Training a model to find patterns in a dataset, typically an unlabeled dataset.
- reinforcement: concerns with how intelligent agents ought to take actions in an environment in order to maximize the notion of cumulative reward

### Deep Learning:
- neuron: A node in a neural network, typically taking in multiple input values and generating one output value
- epoch: A full training pass over the entire dataset such that each example has been seen once. Thus, an epoch represents N/batch size training iterations, where N is the total number of examples.
- activation function: A function (for example, ReLU or sigmoid) that takes in the weighted sum of all of the inputs from the previous layer and then generates and passes an output value (typically nonlinear) to the next layer.
- layer: A set of neurons in a neural network that process a set of input features, or the output of those neurons. Input layer, hidden layers, output layer
- Deep neural network: A type of neural network containing multiple hidden layers. A deep neural network is simply a feed-forward network with many hidden layers. Deep models are for generalization.
- Wide neural network: A linear model that typically has many sparse input features. Wider networks can approximate more interactions between input variables. Wide models are used for memorization.  Deep and wide models are ideal for a recommendation application.

## [AI Platform](https://cloud.google.com/ai-platform)

Use AI Platform to train your machine learning models at scale, to host your trained model in the cloud, and to use your model to make predictions about new data.
You can host your trained machine learning models in the cloud and use AI Platform Prediction to infer target values for new data. You can use AI Platform Training to run your TensorFlow, scikit-learn, and XGBoost training applications in the cloud

## [Deep Learning VM](https://cloud.google.com/deep-learning-vm/)

Deep Learning VM Images images are virtual machine images optimized for data science and machine learning tasks. All images come with key ML frameworks and tools pre-installed, and can be used out of the box on instances with GPUs to accelerate your data processing tasks


## [Cloud AutoML](https://cloud.google.com/automl/)

Limited experience to train and make predictions. Full rest api andscalably train for labeling features.

-   Language
    -   Natural language classification : minimum of 10 documents per label, and ideally 100 times more documents for the most common label than for the least common label. The maximum number of documents for training data is 1,000,000.
    -   Translation
-   Vision: Train models from labeled images
    -   Classification
    -   Vision Edge: export custom trained model, ML Kit in mobile 
    -   Image detection: _Object localization_ - Detects multiple objects in an image and provides information about the object and where the object was found in the image.
-   Structured Data: Tables
    -   Online predictions
    -   Batch predictions:  make an asynchronous request for a batch of predictions using the batchPredict method

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

Synchronous speech recognition returns the recognized text for short audio (less than ~1 minute) in the response as soon as it is processed.

Streaming speech recognition allows you to stream audio to Speech-to-Text and receive a stream speech recognition results in real time as the audio is processed. Streaming speech recognition is available via **gRPC** only.

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
- Label detection: objects, locations, etc, sending the contents of the image file as a **base64 encoded string** in the body of your request.
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
