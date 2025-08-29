# EC2 Instance Monitor (AWS Lambda + SNS + EventBridge)

This project monitors EC2 instance state changes (e.g., `running`, `stopped`) and sends notifications via Amazon SNS whenever a change occurs. It uses an AWS Lambda function triggered by Amazon EventBridge rules and leverages boto3 to fetch EC2 metadata (like instance name).


## ✅ Features

- 🔔 Real-time notifications for EC2 instance state changes
- 📬 Email alerts with instance name, ID, state, and region
- 🛠️ Event-driven, no polling required
- 🔐 Uses IAM best practices and environment variables


## 🗉 Architecture

`EC2 Instance → State Change → EventBridge Rule → Lambda Function → SNS Email`

## 🛠️ Setup Instructions

### 1. Create an SNS Topic

 - Go to Amazon SNS → Topics → Create topic
 - Choose Standard, name it (e.g., ec2-state-alerts)
 - Create a subscription with your email address
 - Confirm the subscription via email

### 2. Create the Lambda Function

 - Runtime: Python 3.9+
 - Set environment variable: `SNS_TOPIC_ARN`
 - Attach the IAM permissions for SNS, Cloudwatch, ECReadonly


### 3. Create the EventBridge Rule

 - Go to EventBridge → Rules → Create rule
 - Event source: AWS events
 - Event pattern:

    ```
    {
        "source": ["aws.ec2"],
        "detail-type": ["EC2 Instance State-change Notification"],
        "detail": {
            "state": ["pending", "running", "stopped"]
        }
    }
    ```

## ✅ Real Test

- Go to EC2 Console
- Start or stop an instance
- Wait ~1–2 minutes
- Check your email for the alert

## Output

### Accept SNS Topic

Email received for SNS subscription

<img width="1464" height="648" alt="Untitled" src="https://github.com/user-attachments/assets/e465bead-b3fc-4195-aacb-aace740a4f4a" />

Accepted Success

<img width="1464" height="771" alt="Untitled" src="https://github.com/user-attachments/assets/b1d41418-1f20-4cba-80c3-32f4b5a74365" />


### Pending Alert Email

<img width="1300" height="771" alt="Untitled" src="https://github.com/user-attachments/assets/dfd4a5c5-c5bc-4c19-86b6-92c5469e89e4" />

### Running Alert Email

<img width="1297" height="796" alt="Untitled" src="https://github.com/user-attachments/assets/7ad8b811-1b26-450e-ad0b-e27284b704e1" />

### Stopped Alert Email

<img width="1296" height="798" alt="Untitled" src="https://github.com/user-attachments/assets/67a668ae-ad06-497b-bbb9-0729a44e2f56" />

