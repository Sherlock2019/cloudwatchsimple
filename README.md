# cloudwatchsimple
CW cannary wiki


1/  AWS CloudWatch Cheat Sheet
What is CloudWatch?
AWS CloudWatch is a monitoring and observability service designed for DevOps engineers, developers, site reliability engineers (SREs), and IT managers. It provides data and actionable insights to monitor applications, respond to system-wide performance changes, optimize resource utilization, and get a unified view of operational health.

Key Features of CloudWatch:
Performance Monitoring: Track application and infrastructure performance with metrics like CPU utilization, disk I/O, and more.
Log Management: Collect, monitor, analyze, and store log files from AWS resources, applications, and services.
Alerts and Notifications: Set alarms to notify when particular thresholds are breached, which can trigger automated actions or notifications.
Dashboard Creation: Create customizable, reusable dashboards for real-time monitoring of AWS resources.
Event Monitoring: Use CloudWatch Events to respond to state changes in your AWS resources.
Basic Commands (using AWS CLI):
List metrics: aws cloudwatch list-metrics
Get statistics for a specific metric: aws cloudwatch get-metric-statistics
Put metric data: aws cloudwatch put-metric-data
Create an alarm: aws cloudwatch put-metric-alarm
Common CloudWatch Metrics (Out of the Box):
EC2 Metrics:

CPUUtilization: Percentage of allocated EC2 compute units that are currently in use.
NetworkIn/NetworkOut: The number of bytes sent/received on all network interfaces by the instance.
DiskReadBytes/DiskWriteBytes: The number of bytes read/written to all disk devices by the instance.
StatusCheckFailed_Instance/StatusCheckFailed_System: Reports whether the instance has passed both the instance status check and the system status check.
RDS Metrics:

CPUUtilization: The percentage of CPU utilization.
DatabaseConnections: The number of database connections in use.
FreeableMemory: The amount of available random access memory.
ReadIOPS/WriteIOPS: The average number of disk I/O operations per second.
S3 Metrics:

BucketSizeBytes: The amount of data in bytes stored in a bucket.
NumberOfObjects: The total number of objects stored in a bucket.
Lambda Metrics:

Invocations: The number of times a function is invoked in response to an event or invocation API call.
Duration: The elapsed time between when a function is invoked and when it returns or is terminated.
Errors: The number of invocations that result in a function error.
Throttles: The number of Lambda function invocation attempts that were throttled.
DynamoDB Metrics:

ReadCapacityUnits/WriteCapacityUnits: The number of read/write capacity units consumed by operations.
ProvisionedReadCapacityUnits/ProvisionedWriteCapacityUnits: The number of provisioned read/write capacity units.
ConsumedReadCapacityUnits/ConsumedWriteCapacityUnits: The number of read/write capacity units consumed over the specified time period.
Best Practices:
Set Alarms: Create alarms for critical metrics to stay ahead of potential issues.
Log Analysis: Regularly analyze logs for patterns and anomalies.
Regular Reviews: Periodically review your metrics and alarms to adjust them as your application and infrastructure evolve.
CloudWatch provides a comprehensive view of your AWS environment, making it easier to keep track of performance and operational health. Remember, while CloudWatch offers numerous metrics out of the box, you can also create custom metrics based on your specific needs.


EC2 Actions:

Stop, Terminate, or Reboot EC2 Instances: Automatically stop, terminate, or reboot an Amazon EC2 instance when an alarm threshold is breached. This is useful for controlling costs or managing instances that are behaving anomalously.
Auto Scaling Actions:

Scale an Auto Scaling Group: Trigger scale-out or scale-in actions for an Auto Scaling group, enabling dynamic scaling in response to changing metrics, such as high CPU utilization or increased load.
SNS Notifications:

Send Notification: Send a notification to an Amazon Simple Notification Service (SNS) topic. This can then be forwarded to email addresses, SMS, or other notification systems, enabling teams to react to the alarm quickly.
EC2 Auto Recovery:

Recover an EC2 Instance: Automatically recover an EC2 instance if it becomes impaired due to an underlying hardware failure.
AWS Lambda Functions:

Invoke Lambda Function: Trigger an AWS Lambda function. This provides enormous flexibility, as the Lambda function can then perform a wide variety of tasks, such as executing custom remediation logic, updating other AWS services, or integrating with third-party services.
Systems Manager:

Execute an AWS Systems Manager Automation: Execute a specific Systems Manager Automation document. This is useful for automated remediation actions on AWS resources.
AWS EBS Volume Actions:

Create EBS Volume Snapshot: Automatically create a snapshot of an Amazon EBS volume when a threshold is breached, which can be vital for data backup and recovery strategies.
Custom Actions:

Custom/Composite Alarms: Trigger a composite alarm or other custom actions based on the specific criteria or combination of several alarms.
Billing Alarms:

Billing Alerts: Trigger an action when your AWS account billing exceeds certain thresholds. This can help manage costs and avoid unexpected charges.
Best Practices for Alarm Actions:
Use SNS for Notifications: Ensure that critical alarms send notifications to an SNS topic with appropriate subscribers.
Automate Remediation: Wherever possible, use Lambda functions or Systems Manager Automations to automatically remediate known issues.
Test Alarm Actions: Regularly test your alarm actions to ensure they work as expected.
Combine with CloudWatch Events/EventBridge: For more complex workflows, combine CloudWatch alarms with AWS EventBridge (formerly CloudWatch Events) to initiate workflows in response to state changes in AWS resources.
Each action can be configured directly in the CloudWatch console when creating or editing an alarm. The choice of action typically depends on the nature of the monitored metric and the desired response to a particular state change.

2 / Canary 

Using a Puppeteer script, like the one provided for a CloudWatch Synthetics Canary, typically does not involve direct interaction through a Chrome browser interface. Instead, Puppeteer operates "headless" by default – it automates a Chrome (or Chromium) browser in the background, without a graphical user interface. However, you can also run Puppeteer in "headed" mode if you need to see the browser interaction visually for debugging purposes.

Here's how you would use the script:

Running the Script Locally:
Set Up Node.js: Ensure that Node.js is installed on your computer. You can download it from Node.js official website.

Install Puppeteer: Create a new Node.js project and install Puppeteer. You can do this by running:

```
mkdir my-puppeteer-project
cd my-puppeteer-project
npm init -y
npm install puppeteer

```

Add the Script: Create a new file, e.g., canary.js, in your project folder and paste the provided Node.js script into this file.

  Run the Script: Execute the script with Node.js:

```
node canary.js
```
  This will run the script using Puppeteer, which will programmatically control a headless Chrome browser to perform the actions defined in the script.

Running the Script in Headed Mode:
If you want to see the browser actions as they occur (useful for debugging), modify the Puppeteer launch options in the script:

```
browser = await puppeteer.launch({ headless: false }); // Set headless to false
This will open a Chrome window and visually show all the operations the script performs.
```
  
Deploying as a CloudWatch Synthetics Canary:
To use the script as a Canary in AWS CloudWatch:

Package the Script: You need to package the script along with its node_modules (dependencies) into a .zip file. This zip file will be used to create the Canary.

Create the Canary in AWS CloudWatch Synthetics:

Go to the AWS Management Console.
Navigate to CloudWatch and then to the Synthetics page.
Create a new Canary and upload the zip file as the source code.
Set the runtime to Node.js, and specify the script entry point (e.g., canary.js).
Configure and Run the Canary: Set up the frequency of the canary runs, any necessary alarms, and other configurations as needed. Once created, AWS will run the script at the specified intervals, and you can monitor the results directly in CloudWatch.

