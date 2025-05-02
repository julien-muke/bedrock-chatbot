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

## ➡️ Step 4 - Set Up API Gateway

We're will create a REST API, the REST API provides an HTTP endpoint for your Lambda function. API Gateway routes requests to your Lambda function, and then returns the function's response to clients.

1. In the navigation pane search for API Gateway, choose REST API, click "Build"
2. Choose Create API, enter a name `chatbot-api` click "Create API".
3. Once the REST API is created, click on "Create Resource" 
4. Enter a resource, i'll enter `chat`