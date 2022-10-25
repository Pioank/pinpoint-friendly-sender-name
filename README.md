# Pinpoint email friendly sender name

The Friendly-From name is the name that shows how the sender wants to be identified, and is visible in an email client. The Friendly-From address typically includes the email account name and the company email address.

![friendly sender](https://github.com/Pioank/pinpoint-friendly-sender-name/blob/main/assets/friendly-sender-name.png)

### Background

Amazon Pinpoint allows you to configure a friendly sender name on a project level, which can be used across all campaigns and journeys. This behaviour can be limiting as different campaigns and journeys might want to use different friendly sender names depending the use case.

It is possible to change the friendly sender name per campaign and journey but only if the campaign or journey is created programmatically via REST API or SDK - [see here](https://medium.com/@syumak/how-to-change-your-email-identity-from-address-to-use-a-friendly-sender-name-using-amazon-938c8d1dcb0) for more information.

### Solution

The purpose of this solution is to enable marketers to choose different **friendly sender names** per journey without depending on technical resources.

The solution in this GitHub repository uses AWS CloudFormation to deploy an AWS Lambda function. Customers can create Amazon Pinpoint journeys, select the AWS Lambda function as an Amazon Pinpoint custom channel and use the field **Custom Data** to pass the **Friendy Name**, **Message Template** and **From email address**. The AWS Lambda function calls the **SendMessages** API operation to send emails to the respective endpoints but with the specified **Friendly Name**.

### Considerations

The proposed solution works only with Amazon Pinpoint Journeys as currently Amazon Pinpoint Campaigns don't support **Custom Data** input from the Pinpoint UI.

### Prerequisites

1. An AWS account
2. An Amazon Pinpoint project
3. Email channel enabled
4. An email template

### How to implement

1. Deploy the [AWS CloudFormation template](https://github.com/Pioank/pinpoint-friendly-sender-name/blob/main/Pinpoint-friendly-sender-name-email.yaml)
2. Navigate to the Amazon Pinpoint console and create a journey
3. Specify the **Journey entry** criteria by either choosing a customer segment or an event
4. Select **Add activity** and choose **Send through a custom channel**
5. Configure the custom channel as shown below. 1) Choose the AWS Lambda function created from the AWS CloudFormation, 2) Under **CustomData** specify the FriendlyName, Message Template Name (in Pinpoint) and email address that you will use to send from the email without commas and separated by comma "," 3) Select **Email** as endpoint type.

![journey configuration](https://github.com/Pioank/pinpoint-friendly-sender-name/blob/main/assets/journey-configuration.png)
