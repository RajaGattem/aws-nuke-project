aws-nuke 

WARNING: 
DO NOT run aws nuke against a production environment. The result of running aws-nuke against an account can be destructive! 
I recommend only using it to clean up dev/test accounts where you don’t mind deleting all resources.

Pre-requisites
A Linux system with AWS CLI installed. 

aws-nuke repository — https://github.com/rebuy-de/aws-nuke (-- References Only )

Configure the AWS CLI profile for AWS Nuke

Step 1: Generate the Access Key ID and Secret Access Key from the AWS IAM Console.
Step 2: Configure the AWS CLI with these credentials using the terminal by running 
aws configure and entering the Access Key ID and Secret Access Key when prompted.

#configure the aws cli
aws configure --profile <profile-name> #Replace with name LIKE fission-nuke



Download and Install aws-nuke
## Download aws-nuke from github
wget -c https://github.com/rebuy-de/aws-nuke/releases/download/v2.16.0/aws-nuke-v2.16.0-linux-amd64.tar.gz

## Extract the aws-nuke binary
tar -xvf aws-nuke-v2.16.0-linux-amd64.tar.gz


## Rename the extracted binary to aws-nuke
mv aws-nuke-v2.16.0-linux-amd64 aws-nuke


## Copy the extracted binary to your $PATH
sudo mv aws-nuke /usr/local/bin/aws-nuke


## Validate
aws-nuke -h



Configure AWS Nuke

-- Need to write config.yaml file to destroy all resources
AWS Nuke explicitly targets resources in the regions defined under regions.
Adding "global" ensures resources from global services (e.g., IAM, CloudFront) are included in the cleanup process.
If you do not include a region where a resource exists, that resource will not be deleted, maintaining safety by default.

config.yaml : 
vim config.yml


regions:
#aws-nuke will look for and remove resources in these specified regions.
  - "ap-south-1"
  - "global" # Resources like global services - cloud front,IAM
account-blocklist: #mandatory* leave as it is.
#Accounts you dont want to delete the resources
- 123456789101 # e.g production account

resource-types: #not mandatory
  #targets:
  # Specific resources you want to remove 
  #- S3Object
  #- S3Bucket
  #- EC2Volume
  
  excludes: #not mandatory
  # Specific resources to exclude from deletion
  - IAMUser
  - IAMGroup
  - IAMRun
  - IAMPolicy
  - IAMGroup                                               
  - IAMUserPolicyAttachment                                
  - IAMGroupPolicy                                         
  - IAMRolesAnywhereProfile                                
  - IAMRolePolicyAttachment                                
  - IAMInstanceProfileRole                                 
  - IAMSAMLProvider                                        
  - IAMRolePolicy                                          
  - IAMUserHTTPSGitCredential                              
  - IAMRolesAnywhereCRL                                    
  - IAMServerCertificate                                   
  - IAMVirtualMFADevice                                    
  - IAMUserMFADevice                                       
  - IAMRole                                                
  - IAMInstanceProfile                                     
  - IAMLoginProfile                                        
  - IAMRolesAnywhereTrustAnchor                            
  - IAMAccountSettingPasswordPolicy                        
  - IAMServiceSpecificCredential                           
  - IAMUser                                                
  - IAMGroupPolicyAttachment                               
  - IAMUserPolicy                                          
  - IAMPolicy                                              
  - IAMUserGroupAttachment                                 
  - IAMUserSSHPublicKey                                    
  - IAMSigningCertificate                                  
  - IAMUserAccessKey                                       
  - IAMOpenIDConnectProvider
accounts:
  “<ACCOUNT_ID>” : {} #Replace with accountID && {} it will apply to all-res





 
Note : 
Command to get all resource-types  $ aws-nuke resource-types
Exclude : 
 Specific resources to exclude from deletion

Destroy all resources in your AWS account using aws-nuke

Commands : 

Before running the aws-nuke command, ensure your AWS account alias is set up correctly:
Go to the IAM Dashboard
Navigate to the Aws Account Alias Section
Create or Edit the Alias –

Verify the nuke ,with alias
We Got an error like this -- if alias is not specified


# To destroy the resources run this command 
aws-nuke run --config config.yaml --profile fission-nuke --no-dry-run



 Give account alias name
After verification, AWS Nuke will automatically delete the resources. Verify whether the resources have been completely destroyed.


 






Future Reference Only for more actions:  Example for destroy all resources
## Download sample config file or below Their is a sample file
curl https://raw.githubusercontent.com/davidokeyode/aws-offensive/main/nuke-config.yml -o nuke-config.yml
## Edit the file as needed - Modify the account ID placeholder and regions
accounts:
  "<ACCOUNT_ID>": {} # aws-nuke-example
## More configuration info/examples for aws-nuke can be found in the main repository here: https://github.com/rebuy-de/aws-nuke






