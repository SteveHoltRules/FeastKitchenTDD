# TDD-Readme
This is a base document for creating TDD

## Purpose
The purpose of this document is to summarize technical requirements captured by Myers-Holum, Inc. Professional Services during the business process mapping phase of the implementation.

This document represents information gathered in meetings held with the internal teams and customer, which enables the Myers-Holum team to map an efficient flow in the NetSuite application.  Once complete, this document will serve as a blueprint for the Myers-Holum team to develop and configure the application, NetSuite version 2021.1.

A series of meetings, focusing on the key areas of the delivery, were used as a starting point for this exercise. This document is inclusive of all custom script requirements provided by the Customer.

## Business Context

Feast Kitchen is a retailer of vegan food subscription boxes currently operating in Europe and California with expansion plans for the broad US market. Feast Kitchen creates unique recipes for weekly meal boxes that are selected by 4000 users on a weekly basis with plans to expand their service to 12,000 boxes. These recipes are planned by chefs, who determine the ingredients and menus. Feast Kitchen will be leveraging NetSuite for purchasing, inventory, and financial reporting. 

## Business Requirement Abstract

Feast Kitchen will be importing customers into NetSuite in order to generate invoices and reconcile payment information. The Customer information will also create the outside benefit of tracking a customers order history over a period of time. This script will create a import of the customer information specified by Feast Kitchen into NetSuite. The script customer location, and the subsidairy will be based on values sent from Feast Kitchen to create a the correct customer record under the correct subsidairy to capture the appropriate address and allocation for tax reconcilliation. 

Currently Feast Kitchen maintains all customer information in an inhouse custom software solution that will be referred to as Kitchen Software. The customer information will be sourced from the Kitchen Software and the customer information will be sent to NetSuite as a JSON file. 

The original scope for the project was to import all the customer records through a CSV. This was determined to be too time consuming, and so Feast Kitchen has changed to prefer a integration to import the customer information. 

There are 10 fields that will come over to NetSuite to make up the customer file. 
Name, Address, .....
There are 6 fields that will require transformation of the data prior to importing the information into a customer record. These fields are...

At every push from the Kitchen Software to NetSuite, the kitchen software will mark the records as success in order to control the data coming into NetSuite. This will be a control point for NetSuite for time responses. In the case that the connnection to NetSuite failes, or times out, then the file will be resent by Feast Kitchen as a part of the next record push. 

Once the record is imported into NetSuite, the script will provide a success response. If there is an error with the upload, Netsuite will provide an error response. If there is a timeout, or a disconnection.

The package size for customers will be broken into batches of xxx. These batches will be loaded onto a customer record. The script will transform the 'transform' fields, and combine the remaining fields onto a customer record. 

Describe the requirement, the GAP in NetSuite, and what is being solved for.
   1.	What is the client’s process and/or background, what is the requirement GAP, what is being solved for?
      - SOW/RTM reference

## Assumptions, Dependencies, and Constraints

Assumptions
Data will be imported based on a schedule from Feast Kitchen from Kitchen Software. The NetSuite script will listen for the data to be sent and will 'create' a customer record. The data will comply with the expectations set forth in this document. The error testing will only include positive test, and data valadation outside of set data types will not be validated by MHI. MHI trusts that Feast Kitchen validates their own database and will protect NetSuite from invalid data, datatypes, and inaccurate information. Testing different data types for each field will be outside of the scope of this integration. Once a customer has been created in NetSuite, the record can be adjusted through the UI, but the changes will not be in sync to Kitchen Software. Therefore, it is a recommended best practice that data that requires change in either system should be manually changed and maintained by Feast Kitchen in both NetSuite and the Kitchen Software in order to maintain consistency.  

In order to remove a customer, the customer will be marked as 'Inactive' through a CSV upload. The Integration will not be able to delete customer records at this point. 

Due to the fact that the customer records will be operational in NetSuite, all custom fields will remain as non-mandatory in the UI. All customer records will be controlled through roles and permission restrictions. 

List key assumptions, dependencies, and constraints for system, process, and customization
   1. Assumptions 
      -	(e.g. calculation rounding)
      - Output expectations, user manual entry, level of automation
      - Out of scope items & exclusionary
    -	Business decisions
   2.	Dependencies/Criteria
      - records, fields, upstream/downstream processes
   3.	Constraints
      - limitations, restrictions, max records, thresholds, execution time 

## Solution Overview
Process Flow – Customer Data Flow
 	
Describe the process flow, insert lucid chart/process map visual
 
Process flow 
  1.	Provide detailed steps and user actions involved in the customization.
      - Write here about detailed sub-steps, user actions
      - Step by step instructions/events for user interaction and expected functionality
      - Provide screenshots where possible

Script Actions 
  1.	What actions does the script need to take?
      - Script actions
      - For an integration, payload field (partner segment : NetSuite record/lists) mapping must be provided

New Fields/forms/items based on this design  
- New fields   
- New Form 
- New Workflow/Script 

Process Flow Alternative Approach 
  •	Describe if another approach could have been taken and why it is not recommended or not feasible for the client

The alternative to importing customers through this integration would be to create customer either through a CSV upload, or manually through the NetSuite UI. Both of these options have been determined to be too labor intensive by the Feast Kitchen team. 

| No. |	NetSuite Field Name | NetSuite Field ID | NS Field Type | Fld Restriction | Kitchen Source Name | Kitchen Source Field ID |
| --- | ------------------- | ----------------- | ------------- | --------------- | ------------------- | ----------------------- |
| 1 | External ID | externalid | Alpha Numeric | Max Size 100 Chars | Customer ID | customerId |
| 2 | Customer ID | entityId | Alpha Numeric  | Max Size 80 Chars | | |	 	 
| 3 | Individual | isperson | Boolean | TRUE/FALSE | | |
| 4 | First Name | firstname | String | Max Size 32 Chars | | |
| 5 | Last Name | lastname | String | Max Size 32 Chars | | |
| 6 | Company Name | companyname | Alpha Numeric | String- Max size 83 | | |
| 7 | Status | status | List | List | | |
| 8 | Subsidiary | subsidiary | List | List | | |
| 9 | Address1_AddressName | Label | String | Sting Max Size 150 | | |
| 10 | Primary Currency | currency | List | List | | |

## Test Case Scenarios
| Test Case |	Test Scenario Description | Test Results |
| --------- | -------------------------- | ------------ |
| 1 | Describe test scenarios | Describe test expected outcome | Success/Failure |
| 2 | Customer Import | Successful Customer Creation, success message sent to Feast Kitchen | Success | 	 	 
| 3 | Customer Import Rejected | Error Message sent to Feast Kitchen  | Success |
| 4 | Customer Field Error | Error Message sent to Feast Kitchen  | Success |
| 5 | Customer Field Error | Error Message sent to Feast Kitchen  | Success |
| 6 | Customer import successful, timeout on response | Resend response?! | Success
| 7 | Customer Import | Fields not mapped | Failure |
| 8 | Customer Import | 