# Getting started with publishing a data product on AWS Data Exchange

**Targeted Audience: Data Providers who are looking to publish their data products on the AWS Data Exchange service.**

#### What is AWS Data Exchange?

> [AWS Data Exchange](https://aws.amazon.com/data-exchange/) is a data marketplace that makes it easy for AWS customers to securely find, subscribe to, and use third-party data in the cloud.

> **If you are a data provider**, AWS Data Exchange (ADX) makes it easy to reach millions of AWS customers migrating to the cloud by removing the need to build and maintain infrastructure for data storage, delivery, billing, and entitlement. ADX also makes it easy for qualified data providers to securely package, license, and deliver data products to millions of AWS customers worldwide.

> **If you are a data subscriber**, you can easily browse the ADX catalog to find hundreds of relevant and up-to-date data products covering a wide range of industries, including financial services, healthcare, life sciences, geospatial, consumer, media & entertainment, and more. Once subscribed to a data product, you can use the ADX API to load data directly into Amazon S3 and then analyze it with a wide variety of AWS analytics and machine learning services.

[Rearc](https://www.rearc.io) is a data provider and one of the ADX launch partners. Products published by Rearc on ADX can be found [here](https://aws.amazon.com/marketplace/search/results?page=1&filters=VendorId&VendorId=a8a86da2-b2d1-4fae-992d-03494e90590b&searchTerms=rearc&category=d5a43d97-558f-4be7-8543-cce265fe6d9d).

If you are looking to make your data available to customers via ADX, it will typically involve (1) sourcing the data, (2) transforming the data to make it useful for consumers, (3) creating a dataset on ADX, (4) creating automatic revisions and, (5) finally publishing the data product. 

In this example, we will walk you through taking a free, publicly available open dataset and publishing it as a data product on ADX. This repository contains all the necessary code and automation to be able to source the data, perform data transformation and interact with ADX to publish a new data product.

## 1. Source the data
As an example, we are using a free, publicly available open dataset for `COVID-19 - World Confirmed Cases, Deaths, and Testing` from [Our World in Data](https://github.com/owid/covid-19-data/tree/master/public/data/). This dataset is updated daily and provides up-to-date data on confirmed cases, deaths, and testing, for the COVID-19 pandemic till date.

Clone this repository. 

#### Directory Layout:

```bash
.
├── pre-processing                  # Data provider side source code files and automation code
│   ├── pre-processing-code         # Source code
│   │   ├── lambda_function.py      # Code to interact with ADX for creating a dataset and revision
│   │   ├── source_data.py          # Code to acquire dataset and perform necessary data transformation
│   │   ├── requirements.txt        # Code dependencies
│   ├── pre-processing-cfn.yaml     # CloudFormation template to setup data provider automation
├── dataset-description.md          # Dataset description that goes on the ADX listing
├── product-description.md          # Product description that goes on the ADX listing
├── init.sh                         # Initialization script to kick-off the automation
├── README.md
└── .gitignore
```

- Source Files: The actual data provider side source code and automation is stored inside the `pre-processing` folder. This folder contains python code to acquire the dataset, perform necessary data transformation and interact with ADX to publish a dataset and a new dataset revision. This folder also stores `pre-processing-cfn.yaml` that is the CloudFormation template to setup the pre-processing-code (python code) to run in AWS Lambda with CloudWatch Scheduled Events rule to trigger AWS Lambda functions periodically.
- Dataset Description: Description of the dataset is stored in `dataset-description.md` file.
- Product Description: Description of the data product is stored in `product-description.md` file.
- Initialization Script: The `init.sh` script triggers the entire workflow as explained below.

#### Prerequisites:
- [Python](https://www.python.org), [Pip](https://pypi.org/project/pip/), [JQ](https://stedolan.github.io/jq/), [AWS CLI V2](https://aws.amazon.com/cli/) and other related developer tools installed and configured on your local developer workstation
- AWS credentials with appropriate permissions to create necessary ADX resources

## 2. Run the init script
Once, you have the pre-processing code written/updated and tested locally, you can run the init shell script to move the pre-processing code to S3, create a dataset on ADX and create the first dataset revision. 

The init script requires following parameters to be passed:
- Source S3 Bucket: where the dataset and pre-processing automation code resides. For Rearc datasets, it's `rearc-data-provider`
- Dataset Name: S3 prefix where the dataset and pre-processing automation code resides. For this example, we are using `covid-19-world-cases-deaths-testing`
- Product Name: product name for the ADX listing. For this example, we are using `COVID-19 - World Confirmed Cases, Deaths, and Testing`
- Product ID: Since, ADX does not provide APIs to programmatically create Products, it can be left blank for now
- Region: AWS region where the product will be listed on ADX. For this example, we are using `us-east-1`

The init script also allows an optional `--profile` parameter to be passed in if you wish to use an alternative set of AWS credentials instead of your default profile.

#### Execute the script as follows: 
`./init.sh --s3-bucket "rearc-data-provider" --dataset-name "covid-19-world-cases-deaths-testing" --product-name "COVID-19 - World Confirmed Cases, Deaths, and Testing" --product-id "blank" --region "us-east-1"`

If the optional profile parameter is needed, add parameter `--profile "rearc-adx-alt"`

The script does the following:
- Zips up contents within the pre-processing folder
- Copies this pre-processing zip file to S3
- Creates a new dataset on ADX
- Creates the pre-processing CloudFormation stack
- Executes the pre-processing Lambda function which acquires the source dataset, copies it to S3 and creates the first revision on ADX
- Destroys the CloudFormation stack

## 3. Publish the product on ADX
At this point, dataset and the first revision is available on ADX for us to create a product. Currently, ADX does not provide APIs to create Products; hence we will have to create a product and link it to the dataset manually using the AWS console. 

Once the product is created, copy the `Product ID` from ADX console and re-run the pre-processing CloudFormation stack by passing all necessary parameters including the product id. Once the CloudFormation stack is successfully created, based on the CloudWatch Scheduled Events rule, pre-processing Lambda function will automatically create new dataset revisions and publish it to ADX!

## Contact/Support Information
- If you find any issues or have enhancements to this process, please open a GitHub [issue](https://github.com/rearc-data/publish-a-data-product-on-aws-data-exchange/issues) and we will gladly take a look at it. Better yet, submit a pull request. Any contributions you make are greatly appreciated :heart:.
- If you are looking for specific open datasets currently not available on ADX, please submit a request on our project board [here](https://github.com/rearc-data/covid-datasets-aws-data-exchange/projects/1).
- If you have any other questions or feedback, send us an email at data@rearc.io.

## About Rearc
Rearc is a cloud, software and services company. We believe that empowering engineers drives innovation. Cloud-native architectures, modern software and data practices, and the ability to safely experiment can enable engineers to realize their full potential. We have partnered with several enterprises and startups to help them achieve agility. Our approach is simple — empower engineers with the best tools possible to make an impact within their industry.
