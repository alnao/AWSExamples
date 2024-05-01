<p align="center">
    <img src="https://img.shields.io/badge/AWS-%23FF9900?style=plastic&logo=AmazonAWS&logoColor=black" style="height:28px;" />
    <img src="https://img.shields.io/badge/Ec2-%23FF9900?style=plastic&logo=amazon-ec2&logoColor=black" style="height:28px;" />
    <img src="https://img.shields.io/badge/Lambda-%23FF9900?style=plastic&logo=AWSlambda&logoColor=black" style="height:28px;" />
    <img src="https://img.shields.io/badge/S3-%23569A31?style=plastic&logo=amazon-s3&logoColor=black" style="height:28px;" />
    <img src="https://img.shields.io/badge/RDS-%23527FFF?style=plastic&logo=amazon-rds&logoColor=black" style="height:28px;" />
    <img src="https://img.shields.io/badge/DynamoDB-%23527FFF?style=plastic&logo=amazon-DynamoDB&logoColor=black" style="height:28px;" />
    <img src="https://img.shields.io/badge/CloudWatch-%23FF4F8B?style=plastic&logo=amazon-cloudwatch&logoColor=black" style="height:28px;" />
    <img src="https://img.shields.io/badge/API Gateway-%23FF4F8B?style=plastic&logo=amazon-API-Gateway&logoColor=black" style="height:28px;" />
    <img src="https://img.shields.io/badge/SQS-%23FF4F8B?style=plastic&logo=amazon-sqs&logoColor=black" style="height:28px;" />
</p>

# Indice
AWS CloudFormation Examples by [AlNao](https://www.alnao.it/aws)

- [Prerequisiti](#Prerequisiti)
- [Lista esempi](#Lista-esempi)
  - [Bucket S3](#Bucket-S3)
- [Esempi in fase di revisione](#Esempi-in-fase-di-revisione)
- [AlNao.it e licenza](#AlNao.it)

# Prerequisiti
- Un account AWS attivo
- La AWS-CLI installata, [documentazione ufficiale](https://docs.aws.amazon.com/it_it/cli/v1/userguide/cli-chap-install.html)
- Utenza tecnica di tipo programmatico configurata su IAM con permessi di esecuzione di CloudFormation e configurazione della AWS-CLI con il comando
    - ```aws configuration```
- La AWS-CLI-SAM installata correttamente, [documentazione ufficiale](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
- Per ogni template, se non indicato diversamente, i comandi da eseguire per eseguire il deploy sono:
  - ```sam validate```
  - ```sam build```
  - ```sam deploy --template-file .\packagedV1.yaml --stack-name <esempio00name> --capabilities CAPABILITY_IAM```
- Se si tratta di template con più files tra build e deploy è indispensabile eseguire il comando di package:
  - ```sam package --output-template-file <packagedV1.yaml> --s3-prefix <repository-path> --s3-bucket <bucket-name>```


# Lista esempi
  

## Bucket S3
* Documentazione [CLI](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/cp.html)
  * Comandi AWS-CLI per creare e distruggere un bucket
    ```
    aws s3 mb s3://bucket-name
    aws s3 ls
    aws s3 rb s3://bucket-name
    ```
  * Comandi AWS-CLI per gestire gli oggetti contenuti in un bucket
    ```
    aws s3 ls bucket-name
    aws s3 mv s3://bucket-name/example.txt s3://bucket-name2/
    aws s3 mv s3://bucket-name/filename.txt ./
    aws s3 cp s3://bucket-name/example.txt s3://my-bucket/
    ```
  * Comandi AWS-CLI per sincronizzare una cartella locale ad un bucket
    ```
    aws s3 sync . s3://my-bucket/path
    ```
  * Svuotare un bucket
    ```
    aws s3 rm s3://my-bucket/path --recursive
    ```
* Documentazione SDK 
  * Esempio Python per la gestione di S3
    ```
    https://github.com/alnao/AwsAlNaoConsole/blob/master/awsPyConsole/sdk/s3_bucket.py
    ```
* Documentazione [CDK in Python](https://docs.aws.amazon.com/it_it/cdk/v2/guide/work-with-cdk-python.html)
  * Esempio Python per creazione di Bucket 
    ```
    https://github.com/alnao/PythonExamples/tree/master/AWSCDK/Cdk01-bucketS3
    ```
* Documentazione [CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket.html)
  ```
  S3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: !Ref NomeBucket
  ```


## Esempi in fase di revisione
- 02 **Istanze EC2**: istanza EC2 con un web-server (compresi user-data, security group, VPC & subnet)
- 03 **Bucket S3 sito**: bucket pubblicamente accessibile e hosted website (senza CloudFront)
- 04 **Lambda con notifica s3**: lambda in Python avviato da una "notifica" da un bucket S3 (senza EventBridge)
- 05 **Condition**: istanza EC2 con creazione di volumi condizionata da un parametro di ambiente (dev/prov)
- 06 **EventBridge**: due regole EventBridge (trigger & cron) per l'invocazioni di Lambda Function 
- 07 **CloudFront**: distribuzione CloudFront che espone un sito statico salavto in un bucket S3
- 08 **Step Function**: definizione di una step function, invocata da un EventBridge-Lambda, i passi eseguiti dalla macchina a stati sono: copia un file, cancellazione del file originale e poi esecuzione di una lambda function
- 09 **Parametri SSM**: uso del template 02 *istanze EC2* ma con un parametro custom recuperato dal servizio SSM
- 10 **Api Gateway**: creazione di un servizio REST, esposto con Api Gateway e Lambda function come back-end
- 11 **Dynamo ApiCrud**: tabella dynamo con micro-servizi per scrivere e leggere nella tabella (con Api Gateway e Lambda function)
- 12 **Lambda Authorizer**: esempio tabella dynamo, CRUD in Lambda Function con in aggiunta una Lambda Authorizer per Api Gateway
- 13 **Lambda Application S3-Utils**: lambda application con lambda function per gestire contenuti S3 (api get, presigned url, excel2csv, unzip, uploader), il template uploader prevede anche un topic-SNS
- 14 **RDS**: creazione di un database MySql con un SecurityGroup dedicato alle regole di accesso
- 15 **Lambda Java**: AWS function sviluppata in linguaggio java e compilata con maven
- 16 **Job Glue**: definizione Job ETL Glue e una step function che esegue logiche per l'invocazione del Job
- 17 **Elastic IP**: definizione di un indirizzo IP con Elastic IP assegnato ad una EC2 creata con l'esempio 02
- 18 **SQS**: definizione di una coda SQS e due Lambda-API producer e consumer
- 19 **EFS**: un file system EFS e una istanza EC2 che monta il volume in automatico nel user-data
- 20 **ECR & ECS**: un repository ECR con un template per avviare ECS (con VPC dedicata e ALB)
- 22: template che crea una VPC e un VPNendpoint da usare con il client da desktop
- 23: template che crea una VPC, un RDS MySql e una EC2, nella EC2 viene installato in automatico un Wordpress
- 25: template che crea un bilanciatore con istanze che eseguono un Wodpress per ciascuna
- 26: template che crea un bilanciatere tra istanze EC2 che caricano un unico EFS e un unico RDS


# License
Nessun contenuto in questo repository è stato creato con IA o automaticamente, tutto il codice è stato scritto con molta pazienza da Alberto Nao. Se il codice è stato preso da altri siti/progetti è sempre indicata la fonte. Per maggior informazioni visitare il sito [alnao.it](https://www.alnao.it/).

Public projects 
<a href="https://it.wikipedia.org/wiki/GNU_General_Public_License"  valign="middle"><img src="https://img.shields.io/badge/License-GNU-blue" style="height:22px;"  valign="middle"></a> 
*Free Software!*
