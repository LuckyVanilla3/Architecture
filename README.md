# Architecture

## Page 35
```shell
yum update -y
yum install -y nginx
systemctl enable nginx
systemctl start nginx
cd /usr/share/nginx/html
echo "<h1>Hello world</h1><h2>This is First Instance</h2>" > index.html
```

## Page 80
```shell
while true; do true; done
```

## Page 167
UserData

```shell
#!/bin/bash
yum update -y
yum install -y nginx
systemctl enable nginx
systemctl start nginx
cd /usr/share/nginx/html
echo "<center><h1>Hello Lucky Vanilla 3</h1><h2>This is Lucky Vanilla 3's shopping mall. </h2><h3>Working very well. Good luck</h3></center>" > index.html
```

## Page 281
```shell
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "[본인 버킷 ARN을 붙여 넣는다]/*"
        }
    ]
}
```

## Page 326
```shell
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EC2FullAccess",
      "Effect": "Allow",
      "Action": [
        "ec2:RunInstances",
        "ec2:TerminateInstances",
        "ec2:CreateImage",
        "ec2:CreateTags",
        "ec2:CreateLaunchTemplate",
        "ec2:Describe*",
        "cloudwatch:GetMetricData",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:ListMetrics"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ELBPermissions",
      "Effect": "Allow",
      "Action": [
        "elasticloadbalancing:CreateLoadBalancer",
        "elasticloadbalancing:CreateTargetGroup",
        "elasticloadbalancing:RegisterTargets",
        "elasticloadbalancing:Describe*",
        "elasticloadbalancing:ModifyTargetGroup",
        "elasticloadbalancing:DeleteTargetGroup",
        "elasticloadbalancing:DeleteLoadBalancer"
      ],
      "Resource": "*"
    },
    {
      "Sid": "AutoScalingPermissions",
      "Effect": "Allow",
      "Action": [
        "autoscaling:CreateAutoScalingGroup",
        "autoscaling:UpdateAutoScalingGroup",
        "autoscaling:DeleteAutoScalingGroup",
        "autoscaling:SetDesiredCapacity",
        "autoscaling:AttachInstances",
        "autoscaling:TerminateInstanceInAutoScalingGroup",
        "autoscaling:CreateLaunchConfiguration",
        "autoscaling:DeleteLaunchConfiguration",
        "autoscaling:Describe*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchAlarms",
      "Effect": "Allow",
      "Action": [
        "cloudwatch:DescribeAlarms",
        "cloudwatch:PutMetricAlarm",
        "cloudwatch:DeleteAlarms"
      ],
      "Resource": "*"
    },
    {
      "Sid": "S3Access",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    },
    {
      "Sid": "S3ListAccess",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::your-bucket-name"
    }
  ]
}
```

## Page 341
```shell
import boto3

# 직접 키를 코드에 입력 (비추천)
aws_access_key = 'YOUR_ACCESS_KEY_ID'
aws_secret_key = 'YOUR_SECRET_ACCESS_KEY'

# 예: S3 클라이언트 생성
s3 = boto3.client(
    's3',
    aws_access_key_id=aws_access_key,
    aws_secret_access_key=aws_secret_key,
    region_name='ap-northeast-2'  # 서울 리전
)

# S3 버킷 리스트 출력
response = s3.list_buckets()
for bucket in response['Buckets']:
    print(bucket['Name'])
```


## Page 347
```shell
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ObjectCRUD",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}


```


## Page 353
```shell
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EC2ReadOnlyWithDateRange",
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "ec2:GetConsoleOutput",
        "ec2:GetPasswordData"
      ],
      "Resource": "*",
      "Condition": {
        "DateGreaterThanEquals": {
          "aws:CurrentTime": "2025-05-15T00:00:00Z"
        },
        "DateLessThanEquals": {
          "aws:CurrentTime": "2025-05-17T23:59:59Z"
        }
      }
    }
  ]
}

```


## Page 382
```shell
cat <<EOF > /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/nginx/access.log",
            "log_group_name": "nginx/access.log",
            "log_stream_name": "{instance_id}",
            "timezone": "UTC"
          },
          {
            "file_path": "/var/log/nginx/error.log",
            "log_group_name": "nginx/error.log",
            "log_stream_name": "{instance_id}",
            "timezone": "UTC"
          }
        ]
      }
    }
  }
}
EOF


```


## Page 419 ~ 420

```shell
import boto3

# DynamoDB 리소스 생성
dynamodb = boto3.resource(
    'dynamodb',
    region_name='ap-northeast-2',  # 리전 지정
    aws_access_key_id='YOUR_ACCESS_KEY',
    aws_secret_access_key='YOUR_SECRET_KEY'
)

table = dynamodb.Table('lucky-table')

```



```shell
response = table.put_item(
   Item={
        'user': 'luckyuser',
        'date': '20250525',
        'name': 'John Doe',
        'age': 30,
        'email': 'john@example.com',
        'interests': ['music', 'sports']
    }
)

```



```shell
response = table.get_item(
    Key={
        'user': 'luckyuser'
    }
)
item = response.get('Item', {})
print(item)

```



```shell
response = table.update_item(
    Key={
        'user': 'luckyuser'
    },
    UpdateExpression='SET age = :val1, interests = list_append(interests, :val2)',
    ExpressionAttributeValues={
        ':val1': 31,
        ':val2': ['reading']
    },
    ReturnValues='UPDATED_NEW'
)

```



```shell
response = table.delete_item(
    Key={
        'user': 'luckyuser'
    }
)

```


## Page 428

```shell
Resources:
  LuckyInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-05377cf8cfef186c2
      InstanceType: t3.micro
      Tags:
        - Key: "Name"
          Value: "lucky-instance"

```

## Page 434
```shell
Parameters:
  LatestAmiId:
    Type: String
    Default: ami-05377cf8cfef186c2

Resources:  
  LuckyInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref LatestAmiId
      InstanceType: t3.micro
      Tags:
        - Key: "Name"
          Value: "lucky-instance"

Outputs:
  PublicIP:
    Description: 'Public IP address of the newly created EC2 instance'
    Value: !GetAtt LuckyInstance.PublicIp

```


## Page 436 ~ 443
```shell
Parameters:
  AvailabilityZoneA:
    Type: AWS::EC2::AvailabilityZone::Name
    Default: "ap-northeast-2a"
    Description: "Choose AZ1 for your VPC."

  AvailabilityZoneB:
    Type: AWS::EC2::AvailabilityZone::Name
    Default: "ap-northeast-2b"
    Description: "Choose AZ2 for your VPC."


########################################################
# Create-VPC : VPC 
########################################################

Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
      - Key: Name
        Value: !Sub '${AWS::StackName}-VPC'

########################################################
# Create-InternetGateway
########################################################

  InternetGateway:
    Type: "AWS::EC2::InternetGateway"
    Properties:
      Tags:
      - Key: Name
        Value: !Sub '${AWS::StackName}-IGW'

########################################################
# Attach - VPC Gateway 
########################################################

  VPCGatewayAttachment:
    Type: "AWS::EC2::VPCGatewayAttachment"
    Properties:
      InternetGatewayId: !Ref InternetGateway
      VpcId: !Ref VPC        

########################################################
# Create-Public-Subnet: PublicSubnet01,02
########################################################

  PublicSubnet01:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Public Subnet 01
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.0.0/24
      AvailabilityZone: !Ref AvailabilityZoneA
      MapPublicIpOnLaunch: "true"
      Tags:
      - Key: Name
        Value: "lucky-public-subnet1"

  PublicSubnet02:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Public Subnet 02
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.3.0/24
      AvailabilityZone: !Ref AvailabilityZoneB
      MapPublicIpOnLaunch: "true"
      Tags:
      - Key: Name
        Value: "lucky-public-subnet2"

########################################################
# Create-Public-RouteTable
########################################################

  PublicRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-Pub-RT"


########################################################
# Associate-Public-RouteTable: VPC_Private_Subnet_a,b Associate VPC_Private_RouteTable 
########################################################

  PublicSubnet01RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet01
      RouteTableId: !Ref PublicRouteTable

  PublicSubnet02RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet02
      RouteTableId: !Ref PublicRouteTable

########################################################
# Create Public Routing Table
########################################################

  PublicRoute:
    DependsOn: VPCGatewayAttachment
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PublicRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway

########################################################
# Create-NATGateway: NATGATEWAY01,02
########################################################

  NatGateway01:
    DependsOn:
    - NatGatewayEIP1
    - PublicSubnet01
    - VPCGatewayAttachment
    Type: AWS::EC2::NatGateway
    Properties:
      AllocationId: !GetAtt "NatGatewayEIP1.AllocationId"
      SubnetId: !Ref PublicSubnet01
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-NAW1"

  NatGatewayEIP1:
    DependsOn:
    - VPCGatewayAttachment
    Type: "AWS::EC2::EIP"
    Properties:
      Domain: vpc
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-EIP1"

  NatGateway02:
    DependsOn:
    - NatGatewayEIP2
    - PublicSubnet02
    - VPCGatewayAttachment
    Type: AWS::EC2::NatGateway
    Properties:
      AllocationId: !GetAtt "NatGatewayEIP2.AllocationId"
      SubnetId: !Ref PublicSubnet01
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-NAW2"

  NatGatewayEIP2:
    DependsOn:
    - VPCGatewayAttachment
    Type: "AWS::EC2::EIP"
    Properties:
      Domain: vpc
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-EIP2"

########################################################
# Create-Private-Subnet: PrivateSubnet01,02
########################################################

  PrivateSubnet01:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Private Subnet 01
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.1.0/24
      AvailabilityZone: !Ref AvailabilityZoneA
      Tags:
      - Key: Name
        Value: "lucky-private-subnet1"

  PrivateSubnet02:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: Private Subnet 02
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.4.0/24
      AvailabilityZone: !Ref AvailabilityZoneB
      Tags:
      - Key: Name
        Value: "lucky-private-subnet2"

########################################################
# Create-Private-RouteTable: PrivateRT01,02
########################################################

  PrivateRouteTable01:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-Pri-RT 1"

  PrivateRouteTable02:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: !Sub "${AWS::StackName}-Pri-RT 2"


########################################################
# Associate-Private-RouteTable: VPC_Private_Subnet_a,b Associate VPC_Private_RouteTable 
########################################################

  PrivateSubnet01RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PrivateSubnet01
      RouteTableId: !Ref PrivateRouteTable01

  PrivateSubnet02RouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PrivateSubnet02
      RouteTableId: !Ref PrivateRouteTable02


########################################################
# Create Private Routing Table
########################################################

  PrivateRoute01:
    DependsOn:
    - VPCGatewayAttachment
    - NatGateway01
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PrivateRouteTable01
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId: !Ref NatGateway01

  PrivateRoute02:
    DependsOn:
    - VPCGatewayAttachment
    - NatGateway02
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PrivateRouteTable02
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId: !Ref NatGateway02

########################################################
# Create-DB-Subnet: DBSubnet01,02
########################################################

  DBSubnet01:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: DB Subnet 01
    Properties:
      VpcId: !Ref VPC
      CidrBlock: "10.0.2.0/24"
      AvailabilityZone: !Ref AvailabilityZoneA
      Tags:
      - Key: Name
        Value: "lucky-db-subnet1"

  DBSubnet02:
    Type: AWS::EC2::Subnet
    Metadata:
      Comment: DB Subnet 02
    Properties:
      VpcId: !Ref VPC
      CidrBlock: "10.0.5.0/24"
      AvailabilityZone: !Ref AvailabilityZoneB
      Tags:
      - Key: Name
        Value: "lucky-db-subnet2"

Outputs:
  VPCId:
    Description: VPC ID
    Value: !Ref VPC
    Export:
      Name: !Sub "${AWS::StackName}-VPCID"

```




