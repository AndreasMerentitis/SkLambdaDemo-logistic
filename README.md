# Serverless Machine Learning on AWS Lambda with scikit-learn

Configured to deploy a scikit-learn model to AWS Lambda using the Serverless framework.
We use a Lambda to downlad the data, another Lambda to prepare the features and train,
and a final Lambda for inference with the trained model. Scikit-learn is loaded from 
a precompiled Lambda layer (which is why deployment is done in us-east-1 for this
example).

by: Andreas Merentitis

### Prerequisites

#### Setup serverless

```  
sudo npm install -g serverless

sudo serverless plugin install -n serverless-python-requirements

pip install -r requirements.txt

```
#### Setup AWS credentials

Make sure you have the AWS access key and secrete keys setup locally, following this video [here](https://www.youtube.com/watch?v=KngM5bfpttA)

### Download the code locally

```  
serverless create --template-url https://github.com/AndreasMerentitis/SkLambdaDemo-logistic --path sk-lambda
```

### Update S3 bucket to unique name
In serverless.yml:
```  
  environment:
    BUCKET: <your_unique_bucket_name> 
```

### Check the file syntax for any files changed 
```
pyflakes infer.py

```
We can ignore the warning about not using 'unzip_requirements' as its needed to set the requirements for lamda


### Deploy to the cloud  

```
cd sk-lambda

npm install

export BUCKET=serverless-ml-1

serverless deploy --stage dev

curl -X POST https://t3r9pasalk.execute-api.eu-west-1.amazonaws.com/dev/upload

curl -X POST https://t3r9pasalk.execute-api.eu-west-1.amazonaws.com/dev/trainsklearn

curl -X POST https://t3r9pasalk.execute-api.eu-west-1.amazonaws.com/dev/infersklearn -d '{"epoch": "1556995767", "input": {"age": ["34"], "workclass": ["Private"], "fnlwgt": ["357145"], "education": ["Bachelors"], "education-num": ["13"], "marital-status": ["Married-civ-spouse"], "occupation": ["Prof-specialty"], "relationship": ["Wife"], "race": ["White"], "sex": ["Female"], "capital-gain": ["0"], "capital-loss": ["0"], "hours-per-week": ["50"], "native-country": ["United-States"], "income": [">50K"]}}'

curl -X POST https://wd24geyt86.execute-api.us-east-1.amazonaws.com/dev/infersklearn -d '{"epoch": "1556995767", "input": {"age": ["44"], "workclass": ["Private"], "fnlwgt": ["357145"], "education": ["Masters"], "education-num": ["13"], "marital-status": ["Never-married"], "occupation": ["Exec-managerial"], "relationship": ["Not-in-family"], "race": ["White"], "sex": ["Male"], "capital-gain": ["15000"], "capital-loss": ["50"], "hours-per-week": ["80"], "native-country": ["United-States"], "income": [">50K"]}}'
```

### Clean up (remove deployment) 


```
aws s3 rm s3://serverless-ml-1 --recursive

serverless remove --stage dev 
```

### Check that the Lambda functions are removed 

```
aws lambda list-functions --region us-east-1
```

# Using data and extending the basic idea from these sources:
* https://github.com/mikepm35/TfLambdaDemo
* https://medium.com/@mike.p.moritz/running-tensorflow-on-aws-lambda-using-serverless-5acf20e00033
* https://www.districtdatalabs.com/building-a-classifier-from-census-data
* https://github.com/mthenw/awesome-layers









