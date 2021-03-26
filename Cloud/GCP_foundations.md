#GCP Foundations markdown by Jellyfish

https://jellyfish.qwiklabs.com/my_learning
	
	1. Introduction
	2. Getting started on GCP
	3. VMs on cloud
	4. Storage
	5. Containers
	6. Applications
	7. Devs
	8. Big Data and ML
	9. Summary

1. Introduction



2. Getting started on GCP

GCP has tiers of management. Infrastructure as a service (IaaS) > Platform as a Service (PaaS) > Managed Services.

Even managed services, we still have responsibility for content, access policy and usage. 

At lowest level of responsibility (IaaS), GCP will look after ops, authentication, Security, etc

Overview
LAMP enables you to quickly start building your websites and applications by providing a coding framework. As a developer, it provides standalone project directories to store your applications. The Bitnami LAMP Stack provides a complete web development environment for Linux that can be launched in one click.
3. VMs on cloud

Console.cloud.google.com - access to the Google Cloud Platform

Compute Engine lets you create and run virtual machines on Google infrastructure. Compute Engine offers scale, performance, and value that lets you easily launch large compute clusters on Google's infrastructure. There are no upfront investments, and you can run thousands of virtual CPUs on a system that offers quick, consistent performance.

From <https://cloud.google.com/compute/docs/> 

Compute engine to start your own VM

4. Storage

https://cloud.google.com/storage

Storage to create new buckets. 



Data transfer to move between buckets (with encryption). 

Big Table (mapreduce/hadoop in the GCP) is a managed service. 
A fully managed, scalable NoSQL database service for large analytical and operational workloads. Bigtable is ideal for storing very large amounts of data in a key-value store and supports high read and write throughput at low latency for fast access to large amounts of data. Throughput scales linearly—you can increase QPS (queries per second) by adding Bigtable nodes. 

https://cloud.google.com/bigtable

Cloud SQL a fully managed relational database service for MySQL, PostgreSQL, and SQL Server.

From <https://cloud.google.com/sql> 

Cloud Spanner is the first scalable, enterprise-grade, globally-distributed, and strongly consistent database service built for the cloud specifically to combine the benefits of relational database structure with non-relational horizontal scale. 

From <https://cloud.google.com/spanner> 

Cloud Firestore is a scalable NoSQL cloud database to store and sync data for client- and server-side development.

From <https://firebase.google.com/docs/firestore> 

Exercise - Getting Started with Cloud Storage and Cloud SQL

Task 1: Sign in to the Google Cloud Platform (GCP) Console
Task 2: Deploy a web server VM instance
Task 3: Create a Cloud Storage bucket using the gsutil command line
Task 4: Create the Cloud SQL instance
Task 5: Configure an application in a Compute Engine instance to use Cloud SQL
Task 6: Configure an application in a Compute Engine instance to use a Cloud Storage object

5. Containers

Containers offer a logical packaging mechanism in which applications can be abstracted from the environment in which they actually run. This decoupling allows container-based applications to be deployed easily and consistently, regardless of whether the target environment is a private data center, the public cloud, or even a developer’s personal laptop. Containerization provides a clean separation of concerns, as developers focus on their application logic and dependencies, while IT operations teams can focus on deployment and management without bothering with application details such as specific software versions and configurations specific to the app. 

Multiple containers are stored in the container registry

From <https://cloud.google.com/containers> 

Container Registry is a single place for your team to manage Docker images, perform vulnerability analysis, and decide who can access what with fine-grained access control. Existing CI/CD integrations let you set up fully automated Docker pipelines to get fast feedback.

From <https://cloud.google.com/container-registry> 

Google Kubenetes Engine is an enterprise-ready containerized solutions with prebuilt deployment templates, featuring portability, simplified licensing, and consolidated billing. These are not just container images, but open source, Google-built, and commercial applications that increase developer productivity, available now on Google Cloud Marketplace.

From <https://cloud.google.com/kubernetes-engine> 

6. API



7. Devs

Google has its own version of Git, called Cloud Source Repositories. These can only accessed from within Google.

Deployment Manager is an infrastructure management service. Create a .ymal file describing your environment and deliver to the deployment manager.

Cloud logging/monitoring is the service for monitoring deployment processes. It contains; Monitoring, Logging, Debug, Error Reporting, Trace, Profiler.  

8. Big Data and ML



Cloud Dataproc is managed Hadoop (Spark/Hive/Pig)

Cloud Dataflow is managed data pipelines (ETL)

Big Query is the managed data warehouse. Query the data lakes using SQL syntax at scale (essentially the same as Athena). Can integrate with Google Docs

Cloud Datalab is the tool for large scale data exploration, analysis and visualisation. Built on Jupyter (analogous to Sagemaker)




Google Vision API -  https://cloud.google.com/vision
