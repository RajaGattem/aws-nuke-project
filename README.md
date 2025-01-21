# AWS Nuke

**AWS Nuke** is a command-line tool that helps you delete all resources in an AWS account. It is designed for cleaning up AWS environments, such as development or test accounts, where you want to remove all resources and start fresh. **Warning**: Do not run AWS Nuke on production accounts or environments, as it will irreversibly delete all resources in the specified account and regions.

---

## **⚠️ Warning: Use with Caution!**

**DO NOT** run AWS Nuke against a **production environment**. The result of running AWS Nuke can be **destructive**, removing all resources in the specified AWS account. 

It is recommended to only use AWS Nuke on **development** or **test** accounts where you don't mind deleting all resources.

---

## **Table of Contents**

- [Pre-requisites](#pre-requisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Destroy Resources](#destroy-resources)
- [Safety Considerations](#safety-considerations)
- [Contributing](#contributing)

---

## **Pre-requisites**

Before using AWS Nuke, make sure you have the following:

1. A **Linux-based system** with AWS CLI installed.
2. An **AWS account** with sufficient permissions to delete resources.
3. **AWS CLI Profile** configured with the proper **Access Key ID** and **Secret Access Key**.

---

## **Installation**

### 1. **Configure the AWS CLI Profile**

Run the following command to configure your AWS CLI with your credentials:

```bash
aws configure --profile <profile-name>
```
Replace `<profile-name>` with a name of your choosing, such as `fission-nuke`.

![Image](https://github.com/user-attachments/assets/af11251b-3a3f-4935-86b3-96d8aebe607a)

### 2. **Download AWS Nuke**

To download AWS Nuke, use `wget` or your preferred method to fetch the latest release:

```bash
wget -c https://github.com/rebuy-de/aws-nuke/releases/download/v2.16.0/aws-nuke-v2.16.0-linux-amd64.tar.gz
```

### 3. **Extract and Install**

After downloading, extract the tarball:

```bash
tar -xvf aws-nuke-v2.16.0-linux-amd64.tar.gz
```

Rename the binary and move it to your system's `PATH`:

```bash
mv aws-nuke-v2.16.0-linux-amd64 aws-nuke
sudo mv aws-nuke /usr/local/bin/aws-nuke
```

### 4. **Validate Installation**

Verify that AWS Nuke is installed correctly:

```bash
aws-nuke -h
```

You should see the help menu, confirming the installation.

---

## **Configuration**

To configure AWS Nuke, you must create a `config.yaml` file that defines the regions and resources to target for deletion.


1. **Clone the GitHub repository:**
   To clone the repository to your local machine, run the following command in your terminal:

   ```bash
   git clone https://github.com/RajaGattem/aws-nuke-project.git
   ```

   This will create a local copy of the repository in a folder named `aws-nuke-project`.

2. **Navigate into the repository folder:**
   After cloning, change into the directory that was created:

   ```bash
   cd aws-nuke-project
   ```

3. **Access the `config.yaml` file:**
   Now that you’re inside the `aws-nuke-project` directory, you can find the `config.yaml` file there. You can open or modify it using any text editor (e.g., `nano`, `vim`, or a GUI editor).

For example:

   ```bash
   vim config.yaml
   ```


### Sample `config.yaml`:

```yaml
regions:
  - "us-east-1"
  - "global"  # Resources like IAM, CloudFront, etc.

account-blocklist:
  - 123456789101  # Exclude specific accounts (e.g., production accounts).

resource-types:
  excludes:
    - IAMUser
    - IAMGroup
    - IAMPolicy
    - IAMRole
    - IAMInstanceProfile

accounts:
  "<ACCOUNT_ID>": {}  # Replace with the actual AWS account ID
```
To replace `<ACCOUNT_ID>` with a **account number** (e.g., `123456789012`) using `sed`, you can modify the `sed` command like this:

### 1. **Basic Command to Replace with Account Number**:

```bash
sed -i 's/<ACCOUNT_ID>/123456789012/g' config.yaml
```
### 2. **Verify the Changes**:
After running the `sed` command, verify that the replacement has been made:

```bash
cat config.yaml
```

This will print the contents of the file, where you should see `<ACCOUNT_ID>` replaced with `123456789012`.

#### Key Sections:
- **regions**: Specifies which AWS regions to target for resource deletion.
- **account-blocklist**: A list of AWS account IDs to exclude from deletion (for safety).
- **resource-types**: Optionally exclude certain resource types from being deleted (e.g., IAM users or roles).
- **accounts**: The AWS account ID that AWS Nuke will target.

### Get All Available Resource Types

To view all available resource types for deletion, run:

```bash
aws-nuke resource-types
```

---

## **Usage**

Once AWS Nuke is installed and your configuration is set, you can start using it.

### 1. **Run AWS Nuke (Dry Run)**

Before actually deleting any resources, it's a good practice to simulate the deletion with the `--no-dry-run` flag. This will show you what resources would be deleted, without actually performing the action:

```bash
aws-nuke run --config config.yaml --profile <profile-name> --no-dry-run
```
![Image](https://github.com/user-attachments/assets/9f8039aa-7882-4d53-b51d-815b1129dcec)

### 2. **Run AWS Nuke (Destructive Mode)**

Once you're ready to destroy the resources, run the following command:

```bash
aws-nuke run --config config.yaml --profile <profile-name> --no-dry-run
```

The `--no-dry-run` flag is necessary to actually delete the resources. **Make sure to double-check your configuration before running this command**, as it will permanently delete the specified resources.

---

## **Destroy Resources**

To delete all resources in your AWS account:

1. Ensure that your AWS account alias is properly set.

![Image](https://github.com/user-attachments/assets/31739d9c-27c0-403c-b207-90d3a2c30e91)

2. Verify that your `config.yaml` file is correctly configured.
3. Run the AWS Nuke command to delete resources as described in the **Usage** section.

---

## **Safety Considerations**

- **Critical Operation**: AWS Nuke **deletes all resources** in the specified AWS account and regions. Use it with extreme caution.
- **Dry-Run Mode**: Always use the `--no-dry-run` option first to verify which resources will be deleted before executing the command.
- **Backup**: Ensure that any critical data (such as S3 buckets or RDS databases) is backed up before running AWS Nuke.

---

## **Contributing**

AWS Nuke is an open-source project, and contributions are welcome! If you find bugs, want to add features, or make improvements, 

---

## **Additional Resources**

- AWS Nuke GitHub Repository: [https://github.com/rebuy-de/aws-nuke](https://github.com/rebuy-de/aws-nuke)
- AWS CLI Documentation: [https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

---
# AWS Services Reference List

This repository contains a list of various AWS services and resources that you may encounter while working with AWS.

## List of AWS Services

- ACMCertificate
- ACMPCACertificateAuthority
- ACMPCACertificateAuthorityState
- AMPWorkspace
- APIGatewayAPIKey
- APIGatewayClientCertificate
- APIGatewayDomainName
- APIGatewayRestAPI
- APIGatewayUsagePlan
- APIGatewayV2API
- APIGatewayV2VpcLink
- APIGatewayVpcLink
- AWSBackupPlan
- AWSBackupRecoveryPoint
- AWSBackupSelection
- AWSBackupVault
- AWSBackupVaultAccessPolicy
- AccessAnalyzer
- AppMeshMesh
- AppMeshRoute
- AppMeshVirtualGateway
- AppMeshVirtualNode
- AppMeshVirtualRouter
- AppMeshVirtualService
- AppStreamDirectoryConfig
- AppStreamFleet
- AppStreamFleetState
- AppStreamImage
- AppStreamImageBuilder
- AppStreamImageBuilderWaiter
- AppStreamStack
- AppStreamStackFleetAttachment
- AppSyncGraphqlAPI
- ApplicationAutoScalingScalableTarget
- ArchiveRule
- AthenaNamedQuery
- AthenaWorkGroup
- AutoScalingGroup
- AutoScalingPlansScalingPlan
- BatchComputeEnvironment
- BatchComputeEnvironmentState
- BatchJobQueue
- BatchJobQueueState
- Cloud9Environment
- CloudDirectoryDirectory
- CloudDirectorySchema
- CloudFormationStack
- CloudFormationStackSet
- CloudFormationType
- CloudFrontDistribution
- CloudFrontDistributionDeployment
- CloudFrontOriginAccessIdentity
- CloudHSMV2Cluster
- CloudHSMV2ClusterHSM
- CloudSearchDomain
- CloudTrailTrail
- CloudWatchAlarm
- CloudWatchDashboard
- CloudWatchEventsRule
- CloudWatchEventsTarget
- CloudWatchLogsDestination
- CloudWatchLogsLogGroup
- CloudWatchLogsResourcePolicy
- CodeBuildProject
- CodeCommitRepository
- CodeDeployApplication
- CodePipelinePipeline
- CodeStarConnection
- CodeStarNotificationRule
- CodeStarProject
- CognitoIdentityPool
- CognitoIdentityProvider
- CognitoUserPool
- CognitoUserPoolClient
- CognitoUserPoolDomain
- ComprehendDocumentClassifier
- ComprehendDominantLanguageDetectionJob
- ComprehendEndpoint
- ComprehendEntitiesDetectionJob
- ComprehendEntityRecognizer
- ComprehendKeyPhrasesDetectionJob
- ComprehendSentimentDetectionJob
- ConfigServiceConfigRule
- ConfigServiceConfigurationRecorder
- ConfigServiceDeliveryChannel
- DAXCluster
- DAXParameterGroup
- DAXSubnetGroup
- DataPipelinePipeline
- DatabaseMigrationServiceCertificate
- DatabaseMigrationServiceEndpoint
- DatabaseMigrationServiceEventSubscription
- DatabaseMigrationServiceReplicationInstance
- DatabaseMigrationServiceReplicationTask
- DatabaseMigrationServiceSubnetGroup
- DeviceFarmProject
- DirectoryServiceDirectory
- DynamoDBTable
- DynamoDBTableItem
- EC2Address
- EC2ClientVpnEndpoint
- EC2ClientVpnEndpointAttachment
- EC2CustomerGateway
- EC2DHCPOption
- EC2EgressOnlyInternetGateway
- EC2Image
- EC2Instance
- EC2InternetGateway
- EC2InternetGatewayAttachment
- EC2KeyPair
- EC2LaunchTemplate
- EC2NATGateway
- EC2NetworkACL
- EC2NetworkInterface
- EC2PlacementGroup
- EC2RouteTable
- EC2SecurityGroup
- EC2Snapshot
- EC2SpotFleetRequest
- EC2Subnet
- EC2TGW
- EC2TGWAttachment
- EC2VPC
- EC2VPCEndpoint
- EC2VPCEndpointServiceConfiguration
- EC2VPCPeeringConnection
- EC2VPNConnection
- EC2VPNGateway
- EC2VPNGatewayAttachment
- EC2Volume
- ECRRepository
- ECSCluster
- ECSClusterInstance
- ECSService
- ECSTaskDefinition
- EFSFileSystem
- EFSMountTarget
- EKSCluster
- EKSFargateProfiles
- EKSNodegroups
- ELB
- ELBv2
- ELBv2TargetGroup
- EMRCluster
- EMRSecurityConfiguration
- ESDomain
- ElasticBeanstalkApplication
- ElasticBeanstalkEnvironment
- ElasticTranscoderPipeline
- ElasticacheCacheCluster
- ElasticacheCacheParameterGroup
- ElasticacheReplicationGroup
- ElasticacheSubnetGroup
- FMSNotificationChannel
- FMSPolicy
- FSxBackup
- FSxFileSystem
- FirehoseDeliveryStream
- GlobalAccelerator
- GlobalAcceleratorEndpointGroup
- GlobalAcceleratorListener
- GlueClassifier
- GlueConnection
- GlueCrawler
- GlueDatabase
- GlueDevEndpoint
- GlueJob
- GlueTrigger
- GuardDutyDetector
- IAMGroup
- IAMGroupPolicy
- IAMGroupPolicyAttachment
- IAMInstanceProfile
- IAMInstanceProfileRole
- IAMLoginProfile
- IAMOpenIDConnectProvider
- IAMPolicy
- IAMRole
- IAMRolePolicy
- IAMRolePolicyAttachment
- IAMSAMLProvider
- IAMServerCertificate
- IAMServiceSpecificCredential
- IAMUser
- IAMUserAccessKey
- IAMUserGroupAttachment
- IAMUserPolicy
- IAMUserPolicyAttachment
- IAMUserSSHPublicKey
- IAMVirtualMFADevice
- ImageBuilderComponent
- ImageBuilderDistributionConfiguration
- ImageBuilderImage
- ImageBuilderInfrastructureConfiguration
- ImageBuilderPipeline
- ImageBuilderRecipe
- IoTAuthorizer
- IoTCACertificate
- IoTCertificate
- IoTJob
- IoTOTAUpdate
- IoTPolicy
- IoTRoleAlias
- IoTStream
- IoTThing
- IoTThingGroup
- IoTThingType
- IoTThingTypeState
- IoTTopicRule
- KMSAlias
- KMSKey
- KinesisAnalyticsApplication
- KinesisStream
- KinesisVideoProject
- LambdaEventSourceMapping
- LambdaFunction
- LambdaLayer
- LaunchConfiguration
- LexBot
- LexIntent
- LexSlotType
- LifecycleHook
- LightsailDisk
- LightsailDomain
- LightsailInstance
- LightsailKeyPair
- LightsailLoadBalancer
- LightsailStaticIP
- MQBroker
- MSKCluster
- MSKConfiguration
- MachineLearningBranchPrediction
- MachineLearningDataSource
- MachineLearningEvaluation
- MachineLearningMLModel
- MediaConvertJobTemplate
- MediaConvertPreset
- MediaConvertQueue
- MediaLiveChannel
- MediaLiveInput
- MediaLiveInputSecurityGroup
- MediaPackageChannel
- MediaPackageOriginEndpoint
- MediaStoreContainer
- MediaStoreDataItems
- MediaTailorConfiguration
- MobileProject
- NeptuneCluster
- NeptuneInstance
- NetpuneSnapshot
- OpsWorksApp
- OpsWorksCMBackup
- OpsWorksCMServer
- OpsWorksCMServerState
- OpsWorksInstance
- OpsWorksLayer
- OpsWorksUserProfile
- RDSDBCluster
- RDSDBClusterParameterGroup
- RDSDBParameterGroup
- RDSDBSubnetGroup
- RDSEventSubscription
- RDSInstance
- RDSOptionGroup
- RDSProxy
- RDSSnapshot
- RedshiftCluster
- RedshiftParameterGroup
- RedshiftSnapshot
- RedshiftSubnetGroup
- RekognitionCollection
- ResourceGroupGroup
- RoboMakerDeploymentJob
- RoboMakerFleet
- RoboMakerRobot
- RoboMakerRobotApplication
- RoboMakerSimulationApplication
- RoboMakerSimulationJob
- Route53HealthCheck
- Route53HostedZone
- Route53ResolverEndpoint
- Route53ResolverRule
- Route53ResourceRecordSet
- Route53TrafficPolicy
- S3Bucket
- S3MultipartUpload
- S3Object
- SESConfigurationSet
- SESIdentity
- SESReceiptFilter
- SESReceiptRuleSet
- SESTemplate
- SFNStateMachine
- SNSEndpoint
- SNSPlatformApplication
- SNSSubscription
- SNSTopic
- SQSQueue
- SSMActivation
- SSMAssociation
- SSMDocument
- SSMMaintenanceWindow
- SSMParameter
- SSMPatchBaseline
- SSMResourceDataSync
- SageMakerApp
- SageMakerDomain
- SageMakerEndpoint
- SageMakerEndpointConfig
- SageMakerModel
- SageMakerNotebookInstance
- SageMakerNotebookInstanceLifecycleConfig
- SageMakerNotebookInstanceState
- SageMakerUserProfiles
- SecretsManagerSecret
- SecurityHub
- ServiceCatalogConstraintPortfolioAttachment
- ServiceCatalogPortfolio
- ServiceCatalogPortfolioProductAttachment
- ServiceCatalogPortfolioShareAttachment
- ServiceCatalogPrincipalPortfolioAttachment
- ServiceCatalogProduct
- ServiceCatalogProvisionedProduct
- ServiceCatalogTagOption
- ServiceCatalogTagOptionPortfolioAttachment
- ServiceDiscoveryInstance
- ServiceDiscoveryNamespace
- ServiceDiscoveryService
- SimpleDBDomain
- StorageGatewayFileShare
- StorageGatewayGateway
- StorageGatewayTape
- StorageGatewayVolume
- TransferServer
- TransferServerUser
- WAFRegionalByteMatchSet
- WAFRegionalByteMatchSetIP
- WAFRegionalIPSet
- WAFRegionalIPSetIP
- WAFRegionalRateBasedRule
- WAFRegionalRateBasedRulePredicate
- WAFRegionalRegexMatchSet
- WAFRegionalRegexMatchTuple
- WAFRegionalRegexPatternSet
- WAFRegionalRegexPatternString
- WAFRegionalRule
- WAFRegionalRuleGroup
- WAFRegionalRulePredicate
- WAFRegionalWebACL
- WAFRegionalWebACLRuleAttachment
- WAFRule
- WAFWebACL
- WAFWebACLRuleAttachment
- WAFv2IPSet
- WAFv2RegexPatternSet
- WAFv2RuleGroup
- WAFv2WebACL
- WorkLinkFleet
- WorkSpacesWorkspace
- XRayGroup
- XRaySamplingRule

