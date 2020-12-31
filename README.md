# Serverless Machine Learning on AWS Lambda with TensorFlow

Configured to deploy a TensorFlow model to AWS Lambda using the Serverless framework.

by: Mike Moritz

updates by: Andreas Merentitis (ubuntu 20.04, py36)

![relative path 6](/bluriness_pie.png?raw=true "bluriness_pie.png")
![relative path 1](/model_train.png?raw=true "model_train.png")

### Prerequisites

#### Setup serverless

```  
sudo npm install -g serverless

sudo serverless plugin install -n serverless-python-requirements

pip install -r requirements.txt

```
#### Setup AWS credentials

Make sure you have AWS access key and secrete keys setup locally, following this video [here](https://www.youtube.com/watch?v=KngM5bfpttA)

### Download the code locally

```  
serverless create --template-url https://github.com/AndreasMerentitis/TfLambdaDemo --path tf-lambda
```

### Update S3 bucket to unique name
In serverless.yml:
```  
  environment:
    BUCKET: <your_unique_bucket_name> 
```


### Deploy to the cloud  


```
cd tf-lambda

npm install

sudo serverless deploy --stage dev

curl -X POST https://t3r9pasalk.execute-api.eu-west-1.amazonaws.com/dev/upload

curl -X POST https://t3r9pasalk.execute-api.eu-west-1.amazonaws.com/dev/train

curl -X POST https://t3r9pasalk.execute-api.eu-west-1.amazonaws.com/dev/infer -d '{"epoch": "1556995767", "input": {"age": ["34"], "workclass": ["Private"], "fnlwgt": ["357145"], "education": ["Bachelors"], "education_num": ["13"], "marital_status": ["Married-civ-spouse"], "occupation": ["Prof-specialty"], "relationship": ["Wife"], "race": ["White"], "gender": ["Female"], "capital_gain": ["0"], "capital_loss": ["0"], "hours_per_week": ["50"], "native_country": ["United-States"], "income_bracket": [">50K"]}}'

curl -X POST https://t3r9pasalk.execute-api.eu-west-1.amazonaws.com/dev/infer -d '{"epoch": "1556995767", "input": {"age": ["34"], "workclass": ["Private"], "fnlwgt": ["357145"], "education": ["Bachelors"], "education-num": ["13"], "marital-status": ["Married-civ-spouse"], "occupation": ["Prof-specialty"], "relationship": ["Wife"], "race": ["White"], "sex": ["Female"], "capital-gain": ["0"], "capital-loss": ["0"], "hours-per-week": ["50"], "native-country": ["United-States"], "income": [">50K"]}}'
```

### Clean up (remove deployment) 


```
aws s3 rm s3://serverless-ml-1 --recursive

sudo serverless remove --stage dev 
```

# Using data and extending the basic idea from these sources:
* https://github.com/mikepm35/TfLambdaDemo
* https://medium.com/@mike.p.moritz/running-tensorflow-on-aws-lambda-using-serverless-5acf20e00033
* https://www.districtdatalabs.com/building-a-classifier-from-census-data
* https://github.com/mthenw/awesome-layers









