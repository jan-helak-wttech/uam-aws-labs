# EKS Notes:

```bash
# Determine default VPC in the region
VPC_ID=$(aws ec2 describe-vpcs --filters Name=isDefault,Values=true --query 'Vpcs[0].VpcId' --output text)
echo $VPC_ID

# List subnets in the default VPC from AZs: us-east-1a, us-east-1b, us-east-1c.
# Record subnets IDs in single variable
SUBNET_IDS=$(aws ec2 describe-subnets --filters Name=vpc-id,Values=$VPC_ID Name=availability-zone,Values=us-east-1a,us-east-1b,us-east-1c --query 'Subnets[*].SubnetId' --output text)
echo $SUBNET_IDS

# Replace spaces with commas in $SUBNET_IDS
SUBNET_IDS=$(echo $SUBNET_IDS | sed 's/ /,/g')
echo $SUBNET_IDS

# Get account ID
ACCOUNT_ID=$(aws sts get-caller-identity --query 'Account' --output text)
echo $ACCOUNT_ID

# Get default ID of the security group
SECURITY_GROUP_ID=$(aws ec2 describe-security-groups --filters Name=vpc-id,Values=$VPC_ID Name=group-name,Values=default --query 'SecurityGroups[0].GroupId' --output text)
echo $SECURITY_GROUP_ID

# Create EKS Cluster
aws eks create-cluster --name uam-lab-eks \
 --role-arn arn:aws:iam::${ACCOUNT_ID}:role/LabRole \
 --resources-vpc-config subnetIds=${SUBNET_IDS},securityGroupIds=${SECURITY_GROUP_ID}

# monitor progress of Cluster creation
watch aws eks describe-cluster --name uam-lab-eks --query cluster.status

# Connect to eks cluser using `aws eks` command:
aws eks --region us-east-1 update-kubeconfig --name uam-lab-eks


# Create Node Group
aws eks create-nodegroup --cluster-name uam-lab-eks \
 --nodegroup-name uam-lab-eks-nodegroup \
 --node-role arn:aws:iam::${ACCOUNT_ID}:role/LabRole \
 --subnets $(echo $SUBNET_IDS | sed 's/,/ /g') \
 --scaling-config minSize=1,maxSize=2,desiredSize=2 \
 --disk-size 20 \
 --instance-types t3.small 

 # monitor progress of node group creation
watch aws eks describe-nodegroup --cluster-name uam-lab-eks --nodegroup-name uam-lab-eks-nodegroup --query nodegroup.status


# Delete Node Group
aws eks delete-nodegroup --cluster-name uam-lab-eks --nodegroup-name uam-lab-eks-nodegroup --region us-east-1
    
# Delete EKS Cluster
aws eks delete-cluster --name uam-lab-eks --region us-east-1
```