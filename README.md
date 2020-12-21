# Use External Python Libraries in AWS Glue Job
AWS Glue is a fully managed ETL (extract, transform, and load) service that makes it simple and cost-effective to categorize your data, clean it, enrich it, and move it reliably between various data stores. Using AWS Glue we can create two types of jobs – 
1.	Python Shell
2.	Spark enabled Job

As AWS glue is fully managed and serverless, we may need to use some external modules or libraries. Below re the detailed steps to use as extension modules and libraries with AWS Glue ETL scripts for both Python and spark enabled jobs.

### Python libraries used in the current Job:
###### Libraries – simple-salesforce (simple-salesforce 0.74.2)
The libraries are imported in different ways in AWS Glue Spark job and AWS Glue Python Shell job – 
1.	Python Shell
* The libraries to be used in the development in an AWS Glue job should be packaged in .egg archive format. If a library consists of a single Python module in one .py file, it can be used directly instead of using archive format.
* Create a new folder and put the libraries to be used inside it.
* Then create a setup.py file in the parent directory with the following contents:

    ```
    from setuptools import setup, find_packages
    setup (
        name = "simple-salesforce",
        version = "0.74.2",
        packages = [' simple-salesforce']
    )
    ```
    *Note: If there are multiple libraries to be archived as .egg, then the folder names of the libraries are to be mentioned in the packages in an array separated by a comma.*

    Example: packages = [‘libraries’, ’comma’, ’separated’]
* To create .egg, you’ll need to do the following from the command line:
    `python setup.py bdist_egg`
* This will generate three new folders:
Build, Dist and foldername-0.1-py2.7.egg -> 2.7 is the version of the python in which the command which creates the .egg is executed.
* Upload the .egg file of the libraries into AWS s3 bucket.
* Open the job on which the external libraries are to be used.
* Click on Action and Edit Job.
* Click on Security configuration, script libraries, and job parameters (optional) and in Python Library Path browse for the zip file in S3 and click save.
![alt text](https://i.ibb.co/PQGv4Kv/adding-library-path.png")
* Open the job and import the packages in the following format
Example: from package import module as myname
![alt text](https://i.ibb.co/q08J1kb/importing-library.png)

2.	Spark enabled Job
The libraries should be packaged simply in .zip archive for Spark enabled jobs instead of .egg.
* Load the zip file of the libraries into s3.
* Open the job on which the external libraries are to be used.
* Click on Action and Edit Job.
* Click on Security configuration, script libraries, and job parameters (optional) and in Python Library Path browse for the zip file in S3 and click save.
![alt text](https://i.ibb.co/DDYhGkD/spark-job.png)
 





