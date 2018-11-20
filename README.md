# aws-twitter-bot
Twitter bot based on Lambda

**Pre requisites:**
* Twitter:
  * Have a twitter account
  * Enable your developer twitter account (https://developer.twitter.com/en/account/get-started)
  * Create your twitter application

Interact with AWS environments and twitter through Python:
* Have Boto3, tweepy, SAM CLI installed to package and deploy the application. 
  * SAM CLI: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-quick-start.html
  * tweepy: ```pip install tweepy```

**Bot 1: Make a bot who retweet automatically the tweets based on keywords**
1. Gather the twitter credentials, generated in the pre requisites
2. Look at the lambda code available in this repository.
3. Create a KMS key (used to encrypt twitter credentials)
4. Create 4 SSM parameters with twitter credentials and use the KMS key
5. Create an S3 bucket to host your code package
6. Create the lambda package
    1. Use SAM and init a project
```sam init --runtime python3.6```
    2. Update template.yaml
    3. Copy/paste the code in app.py
    4. Update app.py with the name of the SSM variables
    5. Update requirements.txt with needed libraries (sample provided in this repository)
    6. Package code, first get the packages updated
    7. ```pip install -r requirements.txt -t hello_world/build/```
    8. ```cp hello_world/*.py hello_world/build/```
    
7. Package the Lambda and put it in your s3 bucket for deployment:
```sam package --template-file template.yaml --output-template-file packaged.yaml --s3-bucket REPLACE_THIS_WITH_YOUR_S3_BUCKET_NAME```
8. Deploy the lambda with a proper stack name and enable IAM capability to create roles automatically
```sam deploy --template-file packaged.yaml --stack-name twitterpollerretweeter --capabilities CAPABILITY_IAM```
9. Let's update the rights in IAM role created for the Lambda and allow access to KMS, SSM parameters
10. Deploy with Alias in template.yaml in order to update the code.
11. If you do some code update, copy code only in build folder and redo sam package and sam deploy

We could instrument a bit this function to leverage X-Ray and understand where we are spending the time:

12. Enable tracing with X-ray. Could do in Lambda console or sam file.
13. Import X-ray sdk in your project
14. Patch_all() will help to patch all boto3 and managed libraries
13. Enable some sub-segment in the code if you want more details.

Sample of instrumented function in the repository.
