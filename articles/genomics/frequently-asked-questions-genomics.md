---
title: 'Microsoft Genomics: Common questions - FAQ | Microsoft Docs'
titleSuffix: Azure
description: Answers to common questions customers ask about Microsoft Genomics. 
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: article
ms.date: 12/07/2017

---
# Microsoft Genomics: Common questions

This article lists the top queries you might have related to Microsoft Genomics. For more information on the Microsoft Genomics service, see [What is Microsoft Genomics?](overview-what-is-genomics.md). For more information about troubleshooting, see our [Troubleshooting Guide](troubleshooting-guide-genomics.md). 

## Does Microsoft Genomics service offer running GATK4 workflows?
Yes. As of October 2018, Microsoft Genomics service offers running GATK4 workflows. Register by **December 15 2018**, to run 20 GATK4 workflows at no cost. Click [here](https://aka.ms/msgatk4 ) to learn more.

## What is the Microsoft Genomics service GATK4 promotion?
Starting **October 2018** The Microsoft Genomics service is offering 20 GATK4 workflows at no cost. This trial ends on **Dec 30th 2018**. [Register](https://aka.ms/msgatk4 ) by **Dec 15, 2018**, to participate in this promotion.

### Common errors you may receive while running Microsoft Genomics with `GATK4-PROMO`.

|**Exception**                                   | **Message**                                                                                                                                                                                    | **Cause**                                                                                                    | **Resolution**                                                                                                                                                                                                       |
|-----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| FeatureDefinitionDisabledForTenantException   | `gatk4-promo` is not enabled for your account. For more information, see https://docs.microsoft.com/en-us/azure/genomics/frequently-asked-questions-genomics                               | You are trying to run `gatk4-promo` workflows with the Microsoft Genomics service without registering for the promotion.       | Please visit [here](https://aka.ms/msgatk4 ) to activate your Microsoft Genomics account for the trial. The trial expires after Dec 30 2018. Registration requests for the trial will be supported until **Dec 15, 2018**. |
| FeatureDefinitionTrialEndedForTenantException | Thank you for trying `gatk4-promo`.Your trial period has ended. For more information, https://docs.microsoft.com/en-us/azure/genomics/frequently-asked-questions-genomics                  | The GATK4 trial has expired on December 30, and you are trying to invoke the `gat4-promo` process_name.  | Switch the process_name parameter to, `gatk4`, instead of `gatk4-promo`. This is the official gatk4 version, and your workflow will be billed if you use this parameter.                                         |
| FeatureMaxWorkflowsQuotaExceededException     | Thank you for trying `gatk4-promo` You have used all of your allocated runs. For more information, see https://docs.microsoft.com/en-us/azure/genomics/frequently-asked-questions-genomics | You have successfully submitted all of your 20 promotional runs for GATK4.                               | Submit any new gatk4 runs with process_name argument set to `gatk4` instead of `gatk4-promo`. Your workflow will be billed when you use this parameter.                                                          |             

## What happens after the GATK4 promotion ends?
You will no longer be able to submit `gatk4-promo` workflows. You can still submit `gatk4` workflows at full price, by setting the `process_name` argument to `gatk4`.

## Can I submit GATK4 workflows without signing up for the promotion?
Yes. You will need to be a current Microsoft Genomics customer. Simply use `gatk` as the `process_name` parameter in the msgen config file. You will be subject to regular price when using this parameter. If you are not a current customer of Microsoft Genomics service, learn more on how to get started by visiting [here](https://docs.microsoft.com/en-us/azure/genomics/quickstart-run-genomics-workflow-portal).

## What is the SLA for Microsoft Genomics?
We guarantee that 99.9% of the time Microsoft Genomics service will be available to receive workflow API requests. For more information, see [SLA](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## How does the usage of Microsoft Genomics show up on my bill?
Microsoft Genomics bills based on the number of gigabases processed per workflow. For more information, see [Pricing](https://azure.microsoft.com/pricing/details/genomics/).


## Where can I find a list of all possible commands and arguments for the `msgen` client?
You can get a full list of available commands and arguments by running `msgen help`. If no further arguments are provided, it shows a list of available help sections, one for each of `submit`, `list`, `cancel`, and `status`. To get help for a specific command, type `msgen help command`; for example, `msgen help submit` lists all of the submit options.

## What are the most commonly used commands for the `msgen` client?
The most commonly used commands are arguments for the `msgen` client include: 

 |**Command**          |  **Field description** |
 |:--------------------|:-------------         |
 |`list`               |Returns a list of jobs you have submitted. For arguments, see `msgen help list`.  |
 |`submit`             |Submits a workflow request to the service. For arguments, see `msgen help submit`.|
 |`status`             |Returns the status of the workflow specified by `--workflow-id`. See also `msgen help status`. |
 |`cancel`             |Sends a request to cancel processing of the workflow specified by `--workflow-id`. See also `msgen help cancel`. |

## Where do I get the value for `--api-url-base`?
Go to Azure portal and open your Genomics account page. Under the **Management** heading, choose **Access keys**. There, you find both the API URL and your access keys.

## Where do I get the value for `--access-key`?
Go to Azure portal and open your Genomics account page. Under the **Management** heading, choose **Access keys**. There, you find both the API URL and your access keys.

## Why do I need two access keys?
You need two access keys in case you want to update (regenerate) them without interrupting usage of the service. For example, you want to update the first key. In that case, you switch all new workflows to using the second key. Then, wait until the already running workflows using the first key are finished. Only then, update the key.

## Do you save my storage account keys?
Your storage account key is used to create short-term access tokens for the Microsoft Genomics service to read your input files and write the output files. The default token duration is 48 hours. The token duration can be changed with the `-sas/--sas-duration` option of the submit command; the value is in hours.

## What genome references can I use?

These references are supported:
 |Reference              | Value of `-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (no alt analysis) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## How do I format my command-line arguments as a config file? 

msgen understands configuration files in the following format:
* All options are provided as key-value pairs with values separated from keys by a colon.
Whitespace is ignored.
* Lines starting with `#` are ignored.
* Any command-line argument in the long format can be converted to a key by stripping its leading dashes and replacing dashes between words with underscores. Here are some conversion examples:

 |Command-line argument            | Configuration file line |
 |:-------------                   |:-------------                 |
 |`-u/--api-url-base https://url`  | *api_url_base:https://url*    |
 |`-k/--access-key KEY`            | *access_key:KEY*              |      
 |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## Next steps

Use the following resources to get started with Microsoft Genomics:
- Get started by running your first workflow through the Microsoft Genomics service. [Run a workflow through the Microsoft Genomics service ](quickstart-run-genomics-workflow-portal.md)
- Submit your own data for processing by the Microsoft Genomics service: [paired FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [Multiple FASTQ or BAM](quickstart-input-multiple.md) 

