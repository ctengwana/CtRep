# CtRep
MyRepository
## The AWS Developer Challenge for Data Engineers
### Instructions

In general feel free to expand/change the challenge in any way you like, just explain your solution and decision making, any concerns around it or limitations etc.

Some information/content is left out intentionally to validate your working experience, please fill in the missing parts.

> Fork this repo with your solution. Ideally, we'd like to see your progression through commits, and don't forget to update the README.md to explain your thought process.

**Please do not reference any websites or published articles. We are looking for original content!**

Please let us know how long the challenge took you. We're not looking for how fast or detailed you are. It's just to give us a clearer idea of what you've produced in the time you decided to take. Feel free to go as big or as small as you want.

### AWS Data Engineer | Challenge Information

This company is ingesting data into a 'data lake' running AWS. Below find a picture of the AWS components used for this company's architecture. 

![AWS-COMPONENTS](image.jpg)

## Section A
### Design an Architecture Diagram - We want to see a picture.
Use the components shown above and create an architecture with your tool of choice (powerpoint / draw.io / etc) showing how to ingest data from an on-premise site into AWS. 

Upload the diagram/picture into the Git repository and indicate where it's saved. Use the given components as far as possible to illustrate your solution.

##### Hints
* Focus on *low cost & high* performance.
* All the data are stored in S3 buckets.
* Athena is used to query the data.
* Show how you will use Step Functions to orchestrate the *ingest* process.
* AWS DynamoDB / CloudWatch must be used.
* Glue Jobs should be used to do the ingest.
* CloudFormation output is a CodeBuild Project
* Git is used to save the terraforms and python / pyspark code.
* You do not have to design/depict the CI/CD, assume that it's already in place.
* Look at the available .tf files to get an understanding of the architecture.
___


## Section B
### Describe the Section A - Architecture Diagram 

Within this Readme.md describe the architecture above, giving detail when and how you would use the selected components.

**Please do not reference any websites or published articles.** 
**We are looking for original content!** 
**We do not want to see definitions of the AWS components.**


##### Hints
* Consider aspects like security (iam / firewalls / vpc / security groups / roles)
* To each component used, give a detailed description on how it fits into the solution, please do not reference existing AWS documentation, use your own words! Feel free to document your thought process. We are interested to find out how you think.
___

## Section C
**Try your best do not keep the section open, we want to see how you apply yourself**

Within the Git Repository Structure:
* Modify the CloudFormation Template [CFN/seed.yaml] in order to create a CodeBuild Project.
* Update the _glue_jobs.tf_, _main.tf_ and _output.tf_ terraform files. 
* Provide the python script that will be used within the glue job, using either python shell or spark, think here about using DynamoDB to keep certain job settings and table structures for S3.
___

## Submit it

Please use a private repo just email us when you are done. 

## Your Solution Starts HERE!!!
Use this area to describe your solution as requested in *_Section B_*. 

## ASSIGNMENT SECTION A ANSWER

In my presentation, there is an on premise VPN which is linked to the AWS cloud via a Direct Connection or via VPN.
Data is ingested into an S3 bucket as raw data.
As soon as the data lands into the S3 bucket, there is Cloudwatch event which sends a notification to an SQS queue. 
Lambda reads from the SQS queues which then checks the Dynamo DB for the job parameters or configurations of the data that has been ingested and then channels it to the correct ETL process.

The Step functions does these steps via the Lambda functions 1) Transforming files formats into efficient file formats e.g. paquet file format.
                                                             2) Moving the files into the operational bucket.
                                                             3) Send a message to the queue.

Another Cloudwatch event triggers the lambda function to check the next queues if anything has been written into the S3 Operational bucket.
Lambda reads from the SQS queues which then checks the Dynamo DB for the job parameters or configurations of the data that has been ingested and then channels it to the correct ETL process and starts the next set of step functions.
The Step functions does these steps via the Lambda functions 1) Calling the Glue jobs to run
                                                             2) The Glue jobs do the heavy transformation of operational data to enhance the data 
                                                             3) Data is then landed into the processed layer
                                                             4) Send a message to the queue.

The crawlers get the metadata from files in S3 and it updates the data catalog which is in turn used by Athena for querying.
Quicksight also uses Athena for dashboards

There is another Lambda function that sends notifictations regarding status of the jobs.
                                                             

## COST SAVING

S3 will have lifecycle rules to move older and infrequently accessed files to cheaper storage options like Glacier or Standard IA.
S3 also has free tier which has free number of reads up to a threshold which also reduces costs.
A Dynamo DB database is used which is serverless hence can manage usage depending on demand which helps in cost cutting e.g use of reserved Instances and it has a threshhold of free reads.
There is use of serverless technology e.g Lambda, Glue and SQS which have free tier hence can reduce costs.

## SECURITY

There is use of the KMS service for encryption in transit e.g during movement from VPN to Cloud.
Use of IAM policies so that users get the least minimum requirement.
Service Manager is used check for compliance violations so that any corrective actions can be taken.
Guard Alert is used to check for attacks like denial of service.


## Additional Comments / Information / Concerns
Add info here.
