# ImageUpdater

## Steps
1. Gather the twitter credentials, generated in the pre requisites
2. Look at the 2 lambda functions code available in this repository.
3. Reuse or create a KMS key (used to encrypt twitter credentials)
4. Reuse or create 4 SSM parameters with twitter credentials and use the KMS key
5. Reuse or create an S3 bucket to host your code package
6. Create the lambda package
    1. Use SAM and init a project
    ```sam init --runtime python2.7```
    2. Update template.yaml with the one in this repository
    3. Copy/paste the code in a file twitterUpdateImage.py
    4. Update the code with the name of the SSM variables
    5. Update requirements.txt with needed libraries (sample provided in this repository)
    6. ```pip install -r requirements.txt -t build/```
    7. Get the pillow.zip archive in the folder upper and decompress it in your build/ folder to have pillow. \ 
    (The library is used to manipulate images.
    8. Copy your python file in build folder: \ 
    ```cp hello_world/*.py build/```
    
7. Package the Lambda and put it in your s3 bucket for deployment: \
```sam package --template-file template.yaml --output-template-file bot2-packaged.yaml --s3-bucket REPLACE_THIS_WITH_YOUR_S3_BUCKET_NAME```
8. Deploy the lambda with a proper stack name and enable IAM capability to create roles automatically \
```sam deploy --template-file packaged.yaml --stack-name bot2-imageupdate --capabilities CAPABILITY_IAM```
9. Let's assign a specific role with access to KMS, SSM, Rekognition and SQS (samples available in folder policies)
10. Configure the trigger of the Lambda to be SQS on queue defined in the Poller.
11. Create a tweet with the keyword #AWSNinja or your if you udpated it
12. Look at the results, the image should be updated.


# Appendix

### Python Virtual environment
**In case you're new to this**, python3 comes with `virtualenv` library by default so you can simply run the following:

1. Create a new virtual environment
2. Install dependencies in the new virtual environment

```bash
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
```


**NOTE:** You can find more information about Virtual Environment at [Python Official Docs here](https://docs.python.org/3/tutorial/venv.html). Alternatively, you may want to look at [Pipenv](https://github.com/pypa/pipenv) as the new way of setting up development workflows
