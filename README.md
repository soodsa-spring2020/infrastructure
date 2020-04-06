# Infrastructure - CSYE 6225

Setting up the networking resources such as Virtual Private Cloud (VPC), Internet Gateway, Route Table and Routes. Using both AWS Command Line Interface and AWS CloudFormation for infrastructure setup and tear down.

## Setup

Create a ```networking.json``` file to setup the network resources using the parameters as specified in the json.

```json
[{
    "ParameterKey": "VpcCIDR",
    "ParameterValue": "10.0.0.0/16"
},
{
    "ParameterKey": "PublicSubnetCIDRs",
    "ParameterValue": "10.0.0.0/24,10.0.1.0/24,10.0.2.0/24"
},
{
    "ParameterKey": "SubnetAvailabilityZones",
    "ParameterValue": "a,b,c"
},
{
    "ParameterKey": "RDSUser",
    "ParameterValue": "master"
},
{
    "ParameterKey": "RDSPassword",
    "ParameterValue": ""
},
{
    "ParameterKey": "RDSDbName",
    "ParameterValue": "csye6225"
},
{
    "ParameterKey": "RDSInstanceClass",
    "ParameterValue": "db.t3.micro"
},
{
    "ParameterKey": "RDSInstanceIdentifier",
    "ParameterValue": "spring2020"
},
{
    "ParameterKey": "RDSAllocatedStorage",
    "ParameterValue": "20"
},
{
    "ParameterKey": "RDSPubliclyAccessible",
    "ParameterValue": "false"
},
{
    "ParameterKey": "RDSMultiAZ",
    "ParameterValue": "false"
},
{
    "ParameterKey": "EC2KeyName",
    "ParameterValue": ""
},
{
    "ParameterKey": "EC2AmiId",
    "ParameterValue": ""
},
{
    "ParameterKey": "EC2InstanceClass",
    "ParameterValue": "t2.micro"
},
{
    "ParameterKey": "EC2SubnetChoice",
    "ParameterValue": "0"
},
{
    "ParameterKey": "EC2VolumeType",
    "ParameterValue": "gp2"
},
{
    "ParameterKey": "EC2DeviceName",
    "ParameterValue": "/dev/sda1"
},
{
    "ParameterKey": "EC2DeleteOnTermination",
    "ParameterValue": "true"
},
{
    "ParameterKey": "EC2VolumeSize",
    "ParameterValue": "20"
}
]
```

## Usage

### Create Stack

```bash
aws cloudformation --profile <PROFILE_NAME> create-stack \
  --stack-name <STACK_NAME> \
  --parameters file://vars-local.json \
  --template-body file://networking.json \
  --region <Region>  \
  --capabilities CAPABILITY_NAMED_IAM
```

### Delete Stack

```bash
aws s3 --profile <PROFILE_NAME> rm s3://{BUCKET_NAME} --recursive

aws cloudformation --profile <PROFILE_NAME> delete-stack \
  --stack-name <STACK_NAME> \
  --region <Region> 
```

### Create/Update SSL Certificate

#### Generate a Private Key
```bash
openssl genrsa 2048 > privatekey.pem
```

#### Request a CSR token
```bash
openssl req -new -key privatekey.pem -out csrtoken.pem
```
#### Upload the CSR token to get the certificate bundle
#### Change the crt file to pem file (optional)
```bash
openssl x509 -in cert-file.crt -outform PEM -out cert-file.pem
```
#### AWS import-certificate
```bash
aws --profile prod acm import-certificate --certificate fileb:/cert-file.pem --certificate-chain fileb://cert-chain.ca-bundle --private-key fileb://privatekey.pem
```
## License
[MIT](https://choosealicense.com/licenses/mit/)
