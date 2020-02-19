# Infrastructure - CSYE 6225

Setting up the networking resources such as Virtual Private Cloud (VPC), Internet Gateway, Route Table and Routes. Using both AWS Command Line Interface and AWS CloudFormation for infrastructure setup and tear down.

## Setup

Create a ```networking.json``` file to setup the network resources using the below parameters.

```json
[
    {
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
    }
]
```

## Usage

### Create Stack

```bash
aws cloudformation --profile PROFILE_NAME create-stack \
  --stack-name STACK_NAME \
  --parameters file://vars.json \
  --template-body file://networking.json
```

### Delete Stack

```bash
aws cloudformation --profile PROFILE_NAME delete-stack \
  --stack-name STACK_NAME 
```

## License
[MIT](https://choosealicense.com/licenses/mit/)
