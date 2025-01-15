# NBA Game Day Notifications / Sports Alerts System

## **Project Overview**
In this project we are building an NBA Notifications system that utilizies the sportsdata.io API to fetch game data including schedules, scores, as well as which arena the game is being played in. This is designed to be run on a scheduled timer to continously fetch the information at the desired intervals. This program has the ability to send notifications in various formats including email, SMS text, and webhook endpoints.
---

## Technologies Used
- **AWS Eventbridge**
- **AWS Lambda**
- **AWS SNS**
- **AWS IAM**
- **SportsData.io API**
- **Python AWS SDK (Boto3)**

---

## **Technical Architecture**
![nba_API](https://gifyu.com/image/Se2hF)

---

## Key Features
- Periodic fetching of NBA game data in real-time.
  - Includes team matchups, schedules, venue, and more.
- Data is automatically sourced and distributed every 3 hours.
- Automated notifications sent directly to phones as SMS text messages, and email. 

---

## Challenges & Solutions
- **SportsData API**: 
  - The documentation for the API was difficult to understand, but after some googling I was able to get the results I wanted.
- **Eventbridge Scheduling**: 
  - The cron format of scheduling was new to me and caused me issues with setting up a schedule correctly.  

---

### **Lessons Learned**
- How to build a notifications system that runs on a scheduled timer.
- How to make sure that the services involved were able to interact with each other through IAM policies and roles, while ensuring Least-Privilege practices.
- How to use Eventbridge to schedule a trigger that will invoke a Lambda function. This uses cron notation.
- How to automate the distribution of alerts to different devices/technologies including SMS, Email and webhooks.
- The concept of event-driven architecture and how it functions in a cloud environment.

---

## Real-World Applications
- **Automated Notifications**: Deliver timely updates to users about dynamic data like NBA game scores, stock prices, product availability, etc.
- **Event Reminders**: Schedule periodic alerts for upcoming events such as meetings, product sales, live broadcasts, etc.
- **Multi-channel Distribution**: Distribute information seamlessly across various channels like SMS and email that can scale to millions of users.

---

## Skills Demonstrated
- Real-Time API Integration
- Automated Workflows
- Python Development
- Least Privilege

---

## How to Get Started

### **Clone the Repository**
```bash
git clone https://github.com/ifeanyiro9/game-day-notifications.git
cd game-day-notifications
```

### **Create an SNS Topic**
1. Open the AWS Management Console.
2. Navigate to the SNS service.
3. Click Create Topic and select Standard as the topic type.
4. Name the topic (e.g., gd_topic) and note the ARN.
5. Click Create Topic.

### **Add Subscriptions to the SNS Topic**
1. After creating the topic, click on the topic name from the list.
2. Navigate to the Subscriptions tab and click Create subscription.
3. Select a Protocol:
- For Email:
  - Choose Email.
  - Enter a valid email address.
- For SMS (phone number):
  - Choose SMS.
  - Enter a valid phone number in international format (e.g., +1234567890).

4. Click Create Subscription.
5. If you added an Email subscription:
- Check the inbox of the provided email address.
- Confirm the subscription by clicking the confirmation link in the email.
6. For SMS, the subscription will be immediately active after creation.

### **Create the SNS Publish Policy**
1. Open the IAM service in the AWS Management Console.
2. Navigate to Policies → Create Policy.
3. Click JSON and paste the JSON policy from gd_sns_policy.json file
4. Replace REGION and ACCOUNT_ID with your AWS region and account ID.
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the policy (e.g., gd_sns_policy).
8. Review and click Create Policy.

### **Create an IAM Role for Lambda**
1. Open the IAM service in the AWS Management Console.
2. Click Roles → Create Role.
3. Select AWS Service and choose Lambda.
4. Attach the following policies:
- SNS Publish Policy (gd_sns_policy) (created in the previous step).
- Lambda Basic Execution Role (AWSLambdaBasicExecutionRole) (an AWS managed policy).
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the role (e.g., gd_role).
8. Review and click Create Role.
9. Copy and save the ARN of the role for use in the Lambda function.

### **Deploy the Lambda Function**
1. Open the AWS Management Console and navigate to the Lambda service.
2. Click Create Function.
3. Select Author from Scratch.
4. Enter a function name (e.g., gd_notifications).
5. Choose Python 3.x as the runtime.
6. Assign the IAM role created earlier (gd_role) to the function.
7. Under the Function Code section:
- Copy the content of the src/gd_notifications.py file from the repository.
- Paste it into the inline code editor.
8. Under the Environment Variables section, add the following:
- NBA_API_KEY: your NBA API key.
- SNS_TOPIC_ARN: the ARN of the SNS topic created earlier.
9. Click Create Function.


### **Set Up Automation with Eventbridge**
1. Navigate to the Eventbridge service in the AWS Management Console.
2. Go to Rules → Create Rule.
3. Select Event Source: Schedule.
4. Set the cron schedule for when you want updates (e.g., hourly).
5. Under Targets, select the Lambda function (gd_notifications) and save the rule.


### **Test the System**
1. Open the Lambda function in the AWS Management Console.
2. Create a test event to simulate execution.
3. Run the function and check CloudWatch Logs for errors.
4. Verify that SMS notifications are sent to the subscribed users.


### **Future Enhancements**
1. Fetch name of the venue instead of ID.
2. Show top performers for games in progress/finished.
3. Complete box score stats across both teams.