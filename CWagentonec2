To automatically install the AWS CloudWatch Logs agent on all your EC2 instances using user data, you need to include a script in the user data field when launching your EC2 instances. This script will run only once when the instance is first started.
the create your default Ami with cloudwatch agent pre installed 



AWS provides different methods to install the CloudWatch Logs agent depending on whether you are using Amazon Linux, Ubuntu, CentOS, or another Linux distribution. Below is a general example using Amazon Linux 2. For other distributions, the installation steps might differ slightly.

Example User Data Script for Amazon Linux 2:
```
#!/bin/bash
# Update the packages on your instance
yum update -y

# Install the CloudWatch Logs agent
yum install -y awslogs

# Configure the awslogs agent to send logs to CloudWatch
cat > /etc/awslogs/awslogs.conf <<- EOF
[general]
state_file = /var/lib/awslogs/agent-state

[/var/log/messages]
file = /var/log/messages
log_group_name = /var/log/messages
log_stream_name = {instance_id}
datetime_format = %b %d %H:%M:%S
EOF

# Set the region (replace "us-west-1" with your desired AWS region)
sed -i 's/us-east-1/us-west-1/' /etc/awslogs/awscli.conf

# Enable and start the CloudWatch Logs agent service
systemctl enable awslogsd
systemctl start awslogsd

```

Explanation:
Update Packages: Updates the instance's package list to ensure you get the latest version of the software.

Install CloudWatch Logs Agent: Installs the awslogs package, which is the CloudWatch Logs agent.

Configure awslogs: Sets up the configuration file for awslogs. The above configuration is for /var/log/messages. You can add additional log files to monitor by adding more sections to this file.

Set AWS Region: Modifies the AWS CLI configuration for awslogs to use the correct AWS region. Replace us-west-1 with the region of your CloudWatch instance.

Enable and Start awslogs Service: Ensures that the CloudWatch Logs agent starts on boot and starts the service immediately.

Using the Script:
When launching an EC2 instance (or an Auto Scaling group), paste this script into the User Data field under Advanced Details.
Make sure that the EC2 instance has an IAM role attached that grants necessary permissions for CloudWatch Logs (e.g., logs:PutLogEvents, logs:CreateLogStream, logs:CreateLogGroup).
Note:
For other Linux distributions like Ubuntu, you may need to modify the package installation commands and configuration file paths.
For Windows instances, the CloudWatch Logs agent installation process is different and involves downloading and running an MSI installer.
AWS now recommends using the CloudWatch unified agent (AmazonCloudWatchAgent) instead of the older CloudWatch Logs agent (awslogs) for enhanced functionality, though the process of automatic installation via user data is similar.
This approach ensures that every time a new instance is launched, it's automatically configured to send specified log data to CloudWatch Logs, improving your ability to monitor and troubleshoot your EC2 instances across your AWS environment.
