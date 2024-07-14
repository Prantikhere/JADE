# JADE - JIL Adapter For DAG Engine

## What is JIL and DAG ?

This document provides a brief explanation of JIL and DAG, two concepts commonly used in data processing and workflow management.

### JIL: Job Intermediate Language

JIL stands for **Job Intermediate Language**. It's a language used to define and describe data processing jobs, specifically within the context of the Apache Hadoop ecosystem. 

**Key features of JIL:**

* **Declarative:** JIL focuses on describing *what* needs to be done rather than *how* to do it. This allows for flexibility and abstraction.
* **Platform-independent:** JIL jobs can be executed on various Hadoop platforms, including Apache Hadoop, Apache Spark, and Apache Flink.
* **Extensible:** JIL supports custom operators and transformations, enabling users to tailor jobs to specific needs.

**Example JIL Job:**

```
job "WordCount" {
  input "input_data" {
    type "text"
    path "/path/to/input/data"
  }
  output "output_data" {
    type "text"
    path "/path/to/output/data"
  }
  transform "WordCounter" {
    input "input_data"
    output "output_data"
  }
}
```

This JIL job defines a word count job with input and output paths and a custom operator "WordCounter".

### DAG: Directed Acyclic Graph

DAG stands for **Directed Acyclic Graph**. It's a type of graph where nodes represent tasks or operations, and edges represent dependencies between them. 

**Key characteristics of DAGs:**

* **Directed:** Edges have direction, indicating the order of execution.
* **Acyclic:** There are no cycles, meaning a task cannot depend on itself directly or indirectly.
* **Workflow Representation:** DAGs are commonly used to represent workflows, where each node represents a step in the process.

**Example DAG:**

```
   A -> B -> C
      \-> D -> E
```

This DAG represents a workflow with five tasks (A, B, C, D, and E). Task A must be completed before tasks B and D. Task B must be completed before task C, and task D must be completed before task E.
This DAG approach is being adoped and implemented in **APACHE AIRFLOW**, for batch job scheduling purpose. 

**Conclusion:**

JIL and DAG are powerful tools for data processing and workflow management. JIL provides a declarative and platform-independent way to define jobs, while DAGs provide a flexible and scalable way to structure and execute complex workflows. Understanding these concepts is crucial for anyone working with large-scale data processing projects.

## How to address this situation ? 
JADE is the proposed solution for this situation 

**Reference Architecture - JADE**

This architecture outlines the workflow for processing Autosys JIL files using a serverless approach on AWS. 
![image](https://github.com/user-attachments/assets/c7f459f8-304b-4c2a-ae4e-cf8edff1aea3)

**Components:**

* **Customer Environment:** The source of Autosys JIL files. 
* **AWS Cloud:** The cloud environment hosting the entire processing pipeline.
* **Virtual private cloud (VPC):** The network environment for the AWS resources.

**Processing Pipeline:**

1. **Extraction of JIL:**
   * JIL files from the customer environment are uploaded to an S3 bucket.
   * The files are processed by a Lambda function, which extracts the JIL metadata and files.
   * Metadata is stored in DynamoDB for easy retrieval and querying.
2. **Conversion:**
   * The JIL files are parsed by a Lambda function to generate key-value pairs.
   * These key-value pairs are stored in an S3 bucket as JSON, XML, or Excel files.
3. **DAG Generator:**
   * A Lambda function generates a DAG (Directed Acyclic Graph) file based on the key-value pairs stored in the S3 bucket.
4. **Deploy DAG files:**
   * The generated DAG file is deployed to an Airflow scheduler running on Amazon EC2 or Fargate (Amazon MWA).
5. **Monitoring:**
   * Amazon CloudWatch monitors the entire process, capturing any issues or errors.
   * An Amazon SNS topic is configured to receive notifications about errors or exceptional files.
   * A Lambda function can be triggered to handle the error files and generate alerts.

**Benefits:**

* **Scalability and Elasticity:** The use of serverless services like Lambda allows for automatic scaling based on demand, ensuring efficient resource utilization.
* **Cost-Effectiveness:**  Serverless architecture eliminates the need to provision and manage servers, reducing operational costs.
* **High Availability:**  The distributed nature of AWS services ensures high availability and resilience.
* **Ease of Management:**  The automation provided by serverless functions simplifies the management of the entire process.

**Use Cases:**

This architecture can be used to process large volumes of Autosys JIL files and deploy them to an Airflow scheduler for automated execution. This can be particularly beneficial for organizations looking to automate their IT operations and improve their efficiency.

**Note:** This architecture is cloud agnostic proven model ,hence it can be adapted based on specific requirements and can be extended to include additional features like data validation and security measures. Code and other details can be shared on request.
