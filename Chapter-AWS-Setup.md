AWS Command Line Tool Installs
Setup ~/.profile variables
Get your EC2 Cert

1) In the box you will manage from, download https://dl.dropboxusercontent.com/u/2273146/aws-tools.zip
Original tools are available from http://aws.amazon.com/developertools as well
Autoscaling, Cloudwatch, ec2, ec2-ami, elasticbeanstalk,elb,iam,rds,ses tools are included in this zip
Install Java on your machine

MAKE YOURSELF AN EC2 KEY
Login to the AWS console
Create a key pair and download the private key


mkdir ~/.aws
cd ~/.aws
openssl genrsa -out initial-aws.pem 2048

# When you are prompted for information in this next step, just
# leave all fields blank.
openssl req -new -x509 -key initial-aws.pem -out cert-aws.pem -days 3650

openssl pkcs8 -topk8 -in initial-aws.pem -nocrypt > aws.pem

Go to Account->Security Credentials
click X.509 Certificates tab
(http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html and http://www.trottercashion.com/2012/05/02/creating-aws-identity-and-access-management-signing-certificates.html have good write ups)
Click on "upload signing certificate" and upload aws.pem
Make your credential files private: chmod 400 ~/*.pem

git clone git@github.com:mozilla/mofo_systems_scripts.git (note for the next step, you will reference this in your fabric commands)

Create an aws_credential_file with this format:
AWSAccessKeyId=AKAIDSKJFALSDKJFLASD
AWSSecretKey=blahblah
# End of aws_credential_file

#Add the following to your ~/.profile (Replace paths as desired)
export AWSAccessKeyId=AKAIDSKJFALSDKJFLASD
export AWSSecretKey=blahblah
export AWS_ACCESS_KEY=AKAIDSKJFALSDKJFLASD
export AWS_SECRET_KEY=blahblah
export AWS_CREDENTIAL_FILE=~/.aws/mozilla/aws_credential_file
export EC2_PRIVATE_KEY="~/.aws/mozilla/aws.pem"
export EC2_CERT="~/.aws/mozilla/cert-aws.pem"

# Fabric
fabfile=/u/mozilla/mofo_systems_scripts/deployment/fabfile.py
alias fab="fab -f /u/mozilla/mofo_systems_scripts/deployment/fabfile.py -i /Users/John/.ssh/jp_mac_id_rsa -u ubuntu -D"

# AWS CLI Directories ###
export AWS_TOOLS=/u/aws-tools
# JP - probably can delete this line
#export AWS_TOOLS_LIB=/usr/local/aws/lib
export JAVA_HOME=/Library/Java/Home

# AWS CLI Region Variables
#  For EC2 and RDS
export EC2_REGION=us-east-1

# AWS CLI Tools
export AWS_EC2_HOME=$AWS_TOOLS/ec2
export EC2_HOME=$AWS_EC2_HOME
export AWS_RDS_HOME=$AWS_TOOLS/rds
export AWS_CLOUDWATCH_HOME=$AWS_TOOLS/cloudwatch
export AWS_ELB_HOME=$AWS_TOOLS/elb
export AWS_IAM_HOME=$AWS_TOOLS/iam
export AWS_AUTO_SCALING_HOME=$AWS_TOOLS/autoscaling
export AWS_CLOUDFORMATION_HOME=$AWS_TOOLS
export AWS_ELASTICACHE_HOME=$AWS_TOOLS
PATH=~/scripts:/opt/vagrant/bin:/usr/local/bin:$AWS_EC2_HOME/bin:$AWS_RDS_HOME/bin:$AWS_CLOUDWATCH_HOME/bin:$AWS_ELB_HOME/bin:$AWS_AUTO_SCALING_HOME/bin:$PATH

## End of your profile add




Here are some useful shortcuts/aliases
## AWS ALIASES ##################
# Get a list of all ec2 instances
alias edi="ec2-describe-instances --hide-tags"
# List of autoscaling groups
alias asd="as-describe-auto-scaling-groups --max-records 3000"
# Scaling Activities
alias didwescale="as-describe-scaling-activities"
# RDS list of db instances
alias rdi="rds-describe-db-instances --max-records 3000"
# A way to kill nodes you hate
# How many are healthy in elb?
alias elbhealth="elb-describe-instance-health"
