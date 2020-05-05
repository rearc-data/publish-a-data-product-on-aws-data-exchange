# Contributing to a Rearc AWS Data Exchange product

🎉🥳 First of all, **THANK YOU** for being interested in contributing to one of our projects 🎉🥳

From the start of our AWS Data Exchange (ADX) initiative we knew we wanted to have an open dialogue with our subscribers. While we try our best to optimize the packaging of our ADX products to be as accommodating as possible, we appreciate there may be use-cases we have overlooked. By contributing to this project, you have an opportunity to improve the usability of the data included with this product not only for yourself, but for anyone who subscribes to this product through ADX. If your contribution is applicable to more than one of our ADX listings, we will gladly improve those other products as well!

You can learn about Rearc’s data services by visiting [https://www.rearc.io/data]( https://www.rearc.io/data).

Below is a set of guidelines for contributing to any of the ADX product repositories available on the [Rearc Data GitHub account](https://github.com/rearc-data).

#### Table of Contents
- [I have a question and/or issue to report. What is the best way to contact Rearc?](#i-have-a-question-andor-issue-to-report-what-is-the-best-way-to-contact-rearc)
- [What should I know before getting started?](#what-should-i-know-before-getting-started)
  * [What is AWS Data Exchange?](#what-is-aws-data-exchange)
  * [Who is Rearc?](#who-is-rearc)
  * [What are Rearc's goals for ADX?](#what-are-rearcs-goals-for-adx)
  * [What is Rearc's philosophy towards dataset formats?](#what-is-rearcs-philosophy-towards-dataset-formats)
  * [What tools are you using throughout your ADX Products?](#what-tools-are-you-using-throughout-your-adx-products)
  * [Directory Layout](#directory-layout)
- [How can I contribute?](#how-can-i-contribute)
  * [Report an Issue/Bug](#report-an-issuebug)
  * [Submit an Improvement/Suggestion](#submit-an-improvementsuggestion)
  * [Pull Request](#pull-request)
- [Additional Resources](#additional-resources)

## I have a question and/or issue to report. What is the best way to contact Rearc?
If you have an issue to report specific to the source code in this repository you can [open a GitHub issue](https://github.com/rearc-data/publish-a-data-product-on-aws-data-exchange/issues). Before opening an issue please review the existing suggestions to see if your idea is already there. If already present, please comment on the existing issue instead of making a new one.

If you have a general inquiry about Rearc's data services you can send an email to data@rearc.io. We would love to hear any suggestion, question or request you may have. 

## What should I know before getting started?

#### What Is AWS Data Exchange?
> [AWS Data Exchange](https://aws.amazon.com/data-exchange/) is a data marketplace that makes it easy for AWS customers to securely find, subscribe to, and use third-party data in the cloud.

#### Who is Rearc?
[Rearc](https://www.rearc.io) is a data provider and one of the launch partners for AWS Data Exchange. Products published by Rearc on ADX can be found [here](https://aws.amazon.com/marketplace/seller-profile?id=a8a86da2-b2d1-4fae-992d-03494e90590b). On ADX we provide a pipeline to automate the (1) sourcing of data, (2) transformation of data to make it useful to subscribers, (3) creation of a dataset for public consumption, (4) automatic revisions of the dataset and, (5) publishing of the data product through ADX.

#### What are Rearc's goals for ADX?
We at Rearc are working tirelessly to lend greater accessibility to interesting and/or important datasets across various disciplines and sources. We realize the direct integration of the ADX, along with other AWS services, facilitates a convenient manner for our subscribers to consume data. For data providers we can supply an automation pipeline, leveraging the AWS platform, to ensure the ubiquity of your data for your consumers.

#### What is Rearc's philosophy towards dataset formats?
We try as much as possible to preserve the integrity of data we provide through ADX, and most of the time this means delivering datasets exactly as they were presented from their source. Sometimes we make minor alterations to datasets to provide wider usability for ADX subscribers (e.g. adjusting CSV files for SQL column naming conventions). For situations where we are unable to maintain the original data file format (often due to a request from the data consumer or a discrepancy in a source's delivery pipeline), we try to limit the extent of needed transformations as much as possible.

#### What tools are you using throughout your ADX Products?
- On our local devices we have chosen to use a combination of [Python 3](https://www.python.org), [Pip](https://pypi.org/project/pip/), [JQ](https://stedolan.github.io/jq/), [AWS CLI V2](https://aws.amazon.com/cli/) and other related developer tools. The specific tools utilized varies on a project-by-project basis. The Python portions of our source code can be adapted into another language with a supported AWS Lambda runtime.
- Most of our ADX projects limit the usage of packages to [The Python Standard Library](https://docs.python.org/3.7/library/index.html). By default, Python provides a robust set of tools to access and manipulate popular data file formats, and we exploit these features to their fullest capacity. Frequent packages we used when interacting with datasets include [os](https://docs.python.org/3.7/library/os.html), [urllib](https://docs.python.org/3.7/library/urllib.html), [json](https://docs.python.org/3.7/library/json.html) and [csv](https://docs.python.org/3.7/library/csv.html). We also utilize [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html), AWS's ASK for Python, which is automatically bundled into AWS Lambda's Python runtimes.
- We are creating various AWS resources throughout the process of deploying and maintaining our ADX projects, including [Data Exchange](https://docs.aws.amazon.com/data-exchange/), [S3](https://docs.aws.amazon.com/s3/), [Lambda](https://docs.aws.amazon.com/lambda/) and [EventBridge](https://docs.aws.amazon.com/eventbridge/). The creation of most of the needed resources is streamlined by the [CloudFormation](https://docs.aws.amazon.com/cloudformation/) template included with this project, ([pre-processing-cfn.yaml](./pre-processing/pre-processing-cfn.yaml)).

#### Directory Layout
While the exact directory layout for one of our ADX projects can vary on a project-by-project basis, a typical presentation of our projects is as follows:
```
.
├── pre-processing                  # Data provider side source code files and automation code
│   ├── pre-processing-code         # Source code
│   │   ├── lambda_function.py      # Code to interact with ADX for creating a dataset and revision
│   │   └── source_data.py          # Code to acquire dataset and perform necessary data transformation
│   └── pre-processing-cfn.yaml     # CloudFormation template to setup data provider automation
├── dataset-description.md          # Dataset description that goes on the ADX listing
├── product-description.md          # Product description that goes on the ADX listing
├── init.sh                         # Initialization script to kick-off the automation
├── README.md
└── .gitignore
```
For more details on the technologies and directory layout used in our ADX products, please visit [Getting started with publishing a data product on AWS Data Exchange](https://github.com/rearc-data/publish-a-data-product-on-aws-data-exchange).

## How can I contribute?

#### Report an Issue/Bug
If you are experiencing an issue with the ADX product featured in this repository, the best way to contact us would be through [opening a GitHub issue](https://github.com/rearc-data/publish-a-data-product-on-aws-data-exchange/issues) in this repository.

When opening an issue please **be as descriptive as possible**. If relevant please **provide information regarding your use-case, development configuration and environment**. The more specific you can be the easier it will be for us to identify and address the situation.

#### Submit an Improvement/Suggestion
If you have a suggestion for improving the ADX product included in this repository you are welcomed to [open a GitHub issue](https://github.com/rearc-data/publish-a-data-product-on-aws-data-exchange/issues) in this repository. While we cannot guarantee every suggestion will be implemented into our products, we will get back to you either way.

If you have any general feedback or suggestions for our ADX products, please contact us at data@rearc.io.

#### Pull Request
One of the major reasons we have made our ADX project repositories public is because we want to better maintain and improve the quality of our data products. We believe our subscribers (you) can play a critical role in this process, and we actively encourage you to fork, branch and open a pull request on this repository. 

Before opening a pull request please familiarize yourself with the [tools](#what-tools-are-you-using-throughout-your-adx-products) and [directory layout](#directory-layout) used in this repository. If you are looking to improve the project's included datasets you should direct yourself to the [`pre-processing/pre-processing-code`](./pre-processing/pre-processing-code) folder, as this is where the gathering and transforming of data occurs.

When you are ready to open a pull request, please **be as descriptive as possible** regarding all improvements you have made. After reviewing your pull request, we may ask you to complete additional changes before your pull request is accepted. If we are unable to accept your pull request, we will make sure to offer context for our decision.

## Additional Resources
- [Rearc Data Homepage](https://www.rearc.io/data)
- Rearc Data Email: data@rearc.io
- [Rearc AWS Marketplace Profile](https://aws.amazon.com/marketplace/seller-profile?id=a8a86da2-b2d1-4fae-992d-03494e90590b)
