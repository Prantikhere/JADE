# JADE
Reference Architecture for JIL to DAG Conversion
## Reference Architecture - JADE

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
