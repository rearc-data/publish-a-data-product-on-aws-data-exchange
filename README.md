## Publish a data product on AWS Data Exchange

This repository describes how to publish a data product on AWS Data Exchange service. For starters, what is AWS Data Exchange?

> [AWS Data Exchange (ADX)](https://aws.amazon.com/data-exchange/) is a data marketplace that makes it easy for AWS customers to securely find, subscribe to, and use third-party data in the cloud.

> **If you are a data provider**, AWS Data Exchange makes it easy to reach the millions of AWS customers migrating to the cloud by removing the need to build and maintain infrastructure for data storage, delivery, billing, and entitling. AWS Data Exchange also makes it easy for qualified data providers to securely package, license, and deliver data products to millions of AWS customers worldwide.

> **If you are a data subscriber**, you can easily browse the AWS Data Exchange catalog to find hundreds of relevant and up-to-date data products covering a wide range of industries, including financial services, healthcare, life sciences, geospatial, consumer, media & entertainment, and more. Once subscribed to a data product, you can use the AWS Data Exchange API to load data directly into Amazon S3 and then analyze it with a wide variety of AWS analytics and machine learning services.

Rearc is a data provider and one of the launch partners with the AWS Data Exchange service. [Here](https://aws.amazon.com/marketplace/search/results?page=1&filters=VendorId&VendorId=a8a86da2-b2d1-4fae-992d-03494e90590b&searchTerms=rearc&category=d5a43d97-558f-4be7-8543-cce265fe6d9d) is a list of data products published by Rearc on AWS Data Exchange service.

If you are a data provider looking to leverage AWS Data Exchange service to sell your data product, it will typically involve sourcing the data, performing necessary data transformation to be able to make the data useful to your consumers, creating a dataset on ADX, creating automatic revisions and finally publishing a data product on ADX. In this example, we take a free, publicly available open dataset and publish a data product on ADX. This repository contains all the necessary code and automation to be able to source the data, perform data transformation and interact with ADX to publish a new data product.

## Getting Started
As explained earlier, in this example we are using a free, publicly available open dataset for `COVID-19 - World Confirmed Cases, Deaths, and Testing` data from [Our World in Data](https://github.com/owid/covid-19-data/tree/master/public/data/). This dataset is a collection of the COVID-19 data. It is updated daily and includes data on confirmed cases, deaths, and testing. It is an up-to-date data on confirmed cases, deaths, and testing, throughout the duration of the COVID-19 pandemic.

#### Directory Layout

```bash
.
├── pre-processing                  # Data provider side source code files and automation code
│   ├── pre-processing-code         # Source code
│   │   ├── lambda_function.py      # Code to interact with ADX for creating a dataset and revision
│   │   ├── source_data.py          # Code to acquire dataset and perform necessary data transformation
│   │   ├── requirements.txt        # Code dependencies
│   ├── pre-processing-cfn.yaml     # CloudFormation code to setup data provider automation
├── dataset-description.md          # Dataset description that goes on ADX
├── product-description.md          # Product description that goes on ADX
├── init.sh                         # Initialization script to start the automation
├── README.md
└── .gitignore
```

- Source Files: The actual data provider side source code and automation code is stored inside the `pre-processing` folder. This folder contains python code to acquire the dataset, perform necessary data transformation and code to interact with ADX service to publish a dataset and a new revision. This folder also stores `pre-processing-cfn.yaml` file that is the CloudFormation code to setup pre-processing-code (python code) to run in AWS Lambda Function with CloudWatch scheduled events rule to trigger AWS Lambda function periodically.
- Dataset Description: The description of the dataset that needs to be created on ADX is stored inside the `dataset-description.md` file.
- Product Description: The description of the data product that needs to be created on ADX is stored inside the `product-description.md` file.
- Initialization Script: The `init.sh` script triggers the entire workflow as explained below.

#### Prerequisites:
- [Python](https://www.python.org), [Pip](https://pypi.org/project/pip/), [JQ](https://stedolan.github.io/jq/), [AWS CLI V2](https://aws.amazon.com/cli/) and other related developer tools installed and configured on your device
- AWS credentials with appropriate permissions to create necessary ADX resources
- Create pre-processing code to acquire source data and perform any necessary data transformation ([source_data.py](./pre-processing/pre-processing-code/source_data.py)) and code to interact with ADX service to publish dataset and a new revision ([lambda_function.py](./pre-processing/pre-processing-code/lambda_function.py))
- Create pre-processing CloudFormation template ([pre-processing-cfn.yaml](./pre-processing/pre-processing-cfn.yaml))
- Create dataset description markdown file ([dataset-description.md](./dataset-description.md))
- Create product markdown file ([product-description.md](./product-description.md))

#### Execute init script
Once, you have the pre-processing code written and tested locally, you can run the init shell script to move the pre-processing code to S3, create dataset on ADX, create the first revision etc. The init script requires following parameters to be passed:

- Source S3 Bucket: This is the source S3 bucket where the dataset and pre-processing automation code resides. For Rearc datasets, it's `rearc-data-provider`
- Dataset Name: This is the S3 prefix where the dataset and pre-processing automation code resides. For this e.g., it's `covid-19-world-cases-deaths-testing`
- Product Name: This is the product name on ADX. For this e.g., it's `COVID-19 - World Confirmed Cases, Deaths, and Testing`
- Product ID: Since, ADX does not provide APIs to programmatically create Products, it can be blank for now
- Region: This is the AWS region where the product will be listed on ADX. For this e.g., it's `us-east-1`

The init script also allows an optional `--profile` parameter to be passed in if you wish to use an alternative set of AWS credentials instead of your default profile.

#### Here is how you can run the init script  
`./init.sh --s3-bucket "rearc-data-provider" --dataset-name "covid-19-world-cases-deaths-testing" --product-name "COVID-19 - World Confirmed Cases, Deaths, and Testing" --product-id "blank" --region "us-east-1"`

#### If the optional profile parameter is needed, add the following:
`--profile "rearc-adx-alt"`

#### At a high-level, init script does following:
- Zips the content of the pre-processing code
- Moves the pre-processing zip file to S3
- Creates a dataset on ADX
- Creates the pre-processing CloudFormation stack
- Executes the pre-processing Lambda function that acquires the source dataset, copies the dataset to S3 and creates the first revision on ADX
- Destroys the CloudFormation stack

#### Publishing the product on ADX
At this point, dataset and the first revision is fully created on ADX. You are now ready to create the new product on ADX. Unfortunately, at this point ADX does not provide APIs to programmatically create Products so, you will have to create the product and link the dataset manually using AWS console. Once, the product is created, grab the `Product ID` from ADX console and re-run the pre-processing CloudFormation stack by passing all necessary parameters including the product id. Once the CloudFormation stack is successfully created, based on the CloudWatch scheduled rules, pre-processing Lambda function will automatically create new dataset revisions and publish it to ADX.

## Contact/Support Information
- If you find any issues or have enhancements with this product, open up a GitHub [issue](https://github.com/rearc-data/publish-a-data-product-on-aws-data-exchange/issues) and we will gladly take a look at it. Better yet, submit a pull request. Any contributions you make are greatly appreciated :heart:.
- If you are interested in any other open datasets, please create a request on our project board [here](https://github.com/rearc-data/covid-datasets-aws-data-exchange/projects/1).
- If you have any other questions or feedback, send us an email at data@rearc.io.

## About Rearc
Rearc is a cloud, software and services company. We believe that empowering engineers drives innovation. Cloud-native architectures, modern software and data practices, and the ability to safely experiment can enable engineers to realize their full potential. We have partnered with several enterprises and startups to help them achieve agility. Our approach is simple — empower engineers with the best tools possible to make an impact within their industry.
