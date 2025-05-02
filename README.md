# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263) 🚀 Build a Serverless AI Chatbot with Amazon Bedrock, Lambda, API Gateway & S3 🤖

<div align="center">

  <br />
    <a href="https://youtu.be/1k6s4shjpRc?si=NOQo77cZQtTA6eGW" target="_blank">
      <img src="https://github.com/user-attachments/assets/cb49ad40-aad7-4901-8fc1-4ae8beae4642" alt="Project Banner">
    </a>
  <br />

<h3 align="center">Serverless AI Chatbot using Amazon Bedrock</h3>

   <div align="center">
     Build this hands-on demo step by step with my detailed tutorial on <a href="http://www.youtube.com/@julienmuke/videos" target="_blank"><b>Julien Muke</b></a> YouTube. Feel free to subscribe 🔔!
    </div>
</div>

## 🚨 Tutorial

This repository contains the steps corresponding to an in-depth tutorial available on my YouTube
channel, <a href="http://www.youtube.com/@julienmuke/videos" target="_blank"><b>Julien Muke</b></a>.

If you prefer visual learning, this is the perfect resource for you. Follow my tutorial to learn how to build projects
like these step-by-step in a beginner-friendly manner!

<a href="https://youtu.be/1k6s4shjpRc?si=NOQo77cZQtTA6eGW" target="_blank"><img src="https://github.com/sujatagunale/EasyRead/assets/151519281/1736fca5-a031-4854-8c09-bc110e3bc16d" /></a>

## <a name="introduction">🤖 Introduction</a>

In this step-by-step tutorial, you'll learn how to create a powerful serverless chatbot using Amazon Bedrock's Titan Text G1 - Express LLM. We’ll connect it to AWS Lambda, expose it via API Gateway with proper CORS handling, and deploy a beautiful HTML/JavaScript frontend using S3 static website hosting.


## <a name="steps">💡 What You'll Learn: </a>
 
• How to use Amazon Bedrock to access large language models (LLMs)<br>
• How to create a chatbot backend with AWS Lambda<br>
• How to connect the chatbot to your frontend using API Gateway<br>
• How to fix CORS errors and handle OPTIONS requests<br>
• How to deploy a stylish chat UI with pure HTML, CSS, and JavaScript<br>

## <a name="steps">🛠 Tech Stack: </a>
• Amazon Bedrock (Titan Text G1 - Express)<br>
• AWS Lambda<br>
• Amazon API Gateway<br>
• Amazon S3 (Static Hosting)<br>
• JavaScript + HTML + CSS<br>

## ➡️ Step 1 - Set Up Amazon Bedrock Access

Make sure your AWS account has Bedrock access (Bedrock is GA now but some regions might differ — N. Virginia us-east-1 is safest).

1. Go to the AWS Console → Amazon Bedrock
2. Request access to `amazon.titan-text-express-v1` you can also choose any models you want to use based on your needs.

![Image](https://github.com/user-attachments/assets/d86d2963-1c01-4fe6-a215-12817e17f1e3)

3. Once approved, you're ready to build

![Image](https://github.com/user-attachments/assets/5d5a4d37-2b96-428a-92c4-01ca441c9f17)

## ➡️ Step 2 - Create an IAM Role for Lambda
To create an IAM role with the console:

1. In the navigation pane search for IAM, choose Roles, and then choose Create role.
2. For Trusted entity type, choose AWS service
3. For Service or use case, choose a service `Lambda` then Choose Next.
4. For Permissions policies, the options depend on the use case that you selected, for this demo select these permissions:
• `AmazonBedrockFullAccess`<br>
• `CloudWatchLogsFullAccess`<br>
5. For Role name, enter `chatBotLambdaRole`
6. Review the role, and then choose Create role.


## ➡️ Step 3 - Create the Lambda Function
To create a Lambda function with the console:

1. In the navigation pane search for Lambda function
2. Choose Create function.
3. Select Author from scratch.
4. In the Basic information pane, for Function name, enter `chatbotlambda`
5. For Runtime, choose Python 3.12 (easiest for Bedrock SDK usage).
6. Leave architecture set to x86_64, and then choose Create function.
7. For the Lambda function code, copy and paste the code below to your lambda code editor:

```py
import boto3
import json

bedrock = boto3.client('bedrock-runtime', region_name='us-east-1')

def lambda_handler(event, context):
    # Handle preflight (OPTIONS) request for CORS
    if event['httpMethod'] == 'OPTIONS':
        return {
            'statusCode': 200,
            'headers': {
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Headers': '*',
                'Access-Control-Allow-Methods': 'OPTIONS,POST,GET'
            },
            'body': json.dumps('Preflight OK')
        }

    # Parse incoming JSON
    body = json.loads(event['body'])
    user_message = body.get('message', '')
    history = body.get('history', [])

    # Optional: construct prompt with history
    conversation = ""
    for turn in history:
        conversation += f"User: {turn['user']}\nAssistant: {turn['assistant']}\n"
    conversation += f"User: {user_message}\nAssistant:"

    # Create payload for Titan
    request_body = {
        "inputText": conversation,
        "textGenerationConfig": {
            "maxTokenCount": 300,
            "temperature": 0.7,
            "topP": 0.9,
            "stopSequences": []
        }
    }

    # Call Titan model
    response = bedrock.invoke_model(
        modelId='amazon.titan-text-express-v1',
        body=json.dumps(request_body),
        contentType='application/json',
        accept='application/json'
    )

    # Extract model output
    result = json.loads(response['body'].read())
    reply = result.get('results', [{}])[0].get('outputText', '')

    # Return response with CORS headers
    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'Access-Control-Allow-Headers': '*',
            'Access-Control-Allow-Methods': 'OPTIONS,POST,GET'
        },
        'body': json.dumps({'response': reply})
    }
```

⚠️Note: This function will:
✅ Accepts a message from the user<br>
✅ Sends it to a Bedrock LLM
✅ Returns the model's response


## ➡️ Step 4 - Set Up API Gateway

We're will create a REST API, the REST API provides an HTTP endpoint for your Lambda function. API Gateway routes requests to your Lambda function, and then returns the function's response to clients.

1. In the navigation pane search for API Gateway, choose REST API, click "Build"
2. Choose Create API, enter a name `chatbot-api` click "Create API".
3. Once the REST API is created, click on "Create Resource" 
4. Enter a resource, i'll enter `chat`
5. Make sure you Enable `CORS` (Cross Origin Resource Sharing), which will create an OPTIONS method that allows all origins, all methods, and several common headers.
6. Once the resource is created, click on "Create method"
7. For the method type, choose `POST` 
8. For the integration type choose "Lambda function"
9. Make sure you Enable Lambda proxy integration to send the request to your Lambda function as a structured event.
10. Choose the your regoin `us-east-1` then choose your existing Lambda function that you created earlier.
11. Keep everything as default then click "Create method"
12. Back resources, click on "Deploy API"

