# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263) 🚀 Build a Serverless AI Chatbot with Amazon Bedrock, Lambda, API Gateway & S3 🤖

<div align="center">

  <br />
    <a href="https://youtu.be/1k6s4shjpRc?si=NOQo77cZQtTA6eGW" target="_blank">
      <img src="https://github.com/user-attachments/assets/cb49ad40-aad7-4901-8fc1-4ae8beae4642" alt="Project Banner">
    </a>
  <br />

<h3 align="center">Deploying a React app using EC2 and Terraform</h3>

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

In this tutorial, you'll learn how to build a fully automated CI/CD pipeline using AWS CodePipeline, CodeBuild, and Amazon S3 to deploy a React.js application hosted on GitHub. Say goodbye to manual deployments, every time you push to your repo, your app will automatically build and deploy to a static website on S3!


## <a name="steps">🔧 How it Works:</a>
 
1. Push code to the `main` branch on GitHub.
2. AWS CodePipeline detects the change.
3. AWS CodeBuild installs dependencies and builds the React app.
4. The build output is deployed to the S3 bucket.
5. S3 serves the app as a public static website.

## ➡️ Step 1 - Setup your React.js App on GitHub