# NEXUS AI - Control Your Tesla from Alexa

NEXUS allows you to control your Tesla using Alexa voice commands by leveraging the Tesla JSON API and AWS Lambda.

---

## Features

- Lock/unlock your Tesla
- Start/stop your Tesla
- Query vehicle status
- Fully voice-controlled through Alexa

---

## Prerequisites

- Node.js (version 20 or higher recommended)
- AWS Account
- Amazon Developer Account
- AWS CLI installed and configured on your local machine

---

## Installation & Setup

### 1. Download and Install Dependencies

- Download the latest release from the Releases page.
- Unzip the archive to your preferred location.
- Open a terminal in that folder and run:

```bash
npm install
````

### 2. Create AWS Lambda Function

* Log into the [AWS Lambda Console](https://console.aws.amazon.com/lambda/).
* Create a new Lambda function with:

  * Blueprint: **Blank Function**
  * Trigger: **Alexa Skills Kit**
  * Name: `TeslaControl`
  * Runtime: Node 4.3 (check for newer supported runtimes, ideally Node 20+)
* Leave the default code in place for now.

### 3. Configure Environment Variables for Lambda

In your Lambda function settings, add these environment variables:

| Variable              | Description                 |
| --------------------- | --------------------------- |
| `TESLA_EMAIL`         | Your Tesla account email    |
| `TESLA_PASS`          | Your Tesla account password |
| `TESLA_VIN`           | Your Tesla vehicle VIN      |
| `TESLA_CLIENT_ID`     | Tesla API Client ID         |
| `TESLA_CLIENT_SECRET` | Tesla API Client Secret     |

### 4. Configure IAM Role and Permissions

* Create a new IAM role named `TeslaControl`.
* Assign the **Simple Microservice** permission template.
* Edit the attached policy and modify the `"Action"` array to:

```json
"Action": [
  "dynamodb:*"
]
```

* Save and validate the policy.

### 5. Create Alexa Skill

* Go to the [Amazon Developer Console](https://developer.amazon.com/alexa/console/ask).
* Create a new skill:

  * Skill type: **Custom Interaction Model**
  * Invocation name: `"my car"` (recommended) or `"my Tesla"`
* In the Interaction Model:

  * Copy `intents.json` into the **Intent Schema**.
  * Copy `utterances.txt` into **Sample Utterances**.
* Create two custom slot types:

  * `LOCK_UNLOCK` with values: `lock`, `unlock`
  * `START_STOP` with values: `start`, `stop`

### 6. Connect Alexa Skill to Lambda

* On the Alexa skill Configuration tab:

  * Choose **AWS Lambda ARN** as the Service Endpoint Type.
  * Region: **North America**
  * Enter your Lambda ARN.
* Enable testing in the **Test** tab.
* Copy the Skill ID (starts with `amzn1.ask.skill.`).

### 7. Add APP\_ID to Lambda Environment Variables

* Add the following environment variable to your Lambda function:

```
APP_ID = Your Alexa Skill ID
```

### 8. Deploy Lambda Function Code

From your terminal, run:

```bash
npm run lambda
```

This will upload your code to the Lambda function.

---

## Testing

Once deployed and connected, test your Alexa skill with commands like:

* "Alexa, tell my car to log in"
* "Alexa, tell my car to get vehicle"

---

## Troubleshooting

* Ensure your Tesla credentials and client secrets are correct.
* Make sure AWS Lambda has internet access to reach Tesla's API.
* Confirm that Alexa Skill and Lambda function APP\_IDs match.
* Verify the Node.js runtime version compatibility.

---

## License

MIT License

---

## Acknowledgments

* Tesla JSON API documentation
* AWS Lambda and Alexa Skills Kit documentation
