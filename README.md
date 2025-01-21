Steps for setting up and using **AWS Nuke** to clean up AWS resources.

---

# AWS Nuke: Destructive Cleanup for AWS Resources

## **Warning:**
**DO NOT** run AWS Nuke against a **production environment**. Running AWS Nuke can be **destructive**, resulting in the deletion of all resources in an AWS account. It is highly recommended only to use AWS Nuke on **development or test environments** where you don't mind deleting all resources.

---

## **Pre-requisites**
Before using AWS Nuke, make sure your system is prepared:

1. **A Linux-based System** with AWS CLI installed.
2. **AWS CLI Profile** configured with valid credentials (Access Key ID and Secret Access Key).

---

## **Step-by-Step Setup**

### **1. Configure the AWS CLI Profile for AWS Nuke**

To use AWS Nuke, configure an AWS CLI profile with the necessary permissions. You need the **Access Key ID** and **Secret Access Key** from the AWS IAM Console.

#### Step 1: **Generate AWS Access Keys**
   - Navigate to the AWS IAM Console and generate a new **Access Key ID** and **Secret Access Key**.

#### Step 2: **Configure AWS CLI with your credentials**

Run the following command and provide your AWS credentials when prompted:

```bash
aws configure --profile <profile-name>
```
*Replace `<profile-name>` with a meaningful name, like `fission-nuke`.*

---

### **2. Download and Install AWS Nuke**

#### Step 1: **Download the AWS Nuke Binary**

Use `wget` to download the binary from the official AWS Nuke GitHub releases page:

```bash
wget -c https://github.com/rebuy-de/aws-nuke/releases/download/v2.16.0/aws-nuke-v2.16.0-linux-amd64.tar.gz
```

#### Step 2: **Extract the Binary**

Extract the downloaded `.tar.gz` file:

```bash
tar -xvf aws-nuke-v2.16.0-linux-amd64.tar.gz
```

#### Step 3: **Rename the Extracted Binary**

Rename the extracted binary for ease of use:

```bash
mv aws-nuke-v2.16.0-linux-amd64 aws-nuke
```

#### Step 4: **Move the Binary to your System's PATH**

Copy the `aws-nuke` binary to `/usr/local/bin/` to make it executable from anywhere:

```bash
sudo mv aws-nuke /usr/local/bin/aws-nuke
```

#### Step 5: **Validate Installation**

Verify that AWS Nuke is installed correctly by running:

```bash
aws-nuke -h
```

This will display the help message, confirming that the installation was successful.

---

### **3. Configure AWS Nuke**

To configure AWS Nuke, you'll need to create a `config.yaml` file that defines the AWS regions and resources to target for deletion.

#### Sample `config.yaml`:

```yaml
regions:
  - "ap-south-1"  # Specify the AWS regions to target for deletion.
  - "global"      # Include global resources like IAM, CloudFront.

account-blocklist:
  - 123456789101  # Don't delete resources from these accounts (e.g., production account).

resource-types:
  excludes:
    - IAMUser           # Exclude specific resources from deletion.
    - IAMGroup
    - IAMPolicy
    - IAMRole
    - IAMInstanceProfile

accounts:
  "<ACCOUNT_ID>": {}  # Replace with your actual AWS account ID.
```

#### Key Sections:

- **regions**: Specifies the regions where AWS Nuke should look for resources to delete. The `global` keyword includes global services (IAM, CloudFront, etc.).
- **account-blocklist**: A list of account IDs to exclude from resource deletion (e.g., production environments).
- **resource-types**: This section defines the types of resources to include or exclude from deletion. You can customize this based on what you want to delete.
- **accounts**: Replace `<ACCOUNT_ID>` with your actual AWS account ID.

#### Get All Available Resource Types:

To view all available resource types for deletion, run the following command:

```bash
aws-nuke resource-types
```

---

### **4. Destroy Resources**

Before running AWS Nuke, ensure your AWS account alias is configured properly.

#### Step 1: **Verify Account Alias**

Go to the **IAM Dashboard** in the AWS Console and navigate to the **Account Alias** section. Create or edit the alias to match your environment.

#### Step 2: **Run AWS Nuke**

To simulate the deletion (dry-run mode), use:

```bash
aws-nuke run --config config.yaml --profile <profile-name> --no-dry-run
```

This command runs AWS Nuke and will delete the resources based on the configuration. 

> **Important**: The `--no-dry-run` flag is required to actually delete the resources. Without this flag, AWS Nuke will only perform a simulation of the deletion process.

---

### **5. Future Reference and Additional Actions**

For further actions, such as downloading a sample configuration or editing your file, refer to the following:

- **Download sample config**:

```bash
curl https://raw.githubusercontent.com/davidokeyode/aws-offensive/main/nuke-config.yml -o nuke-config.yml
```

- Edit the file as needed by replacing `<ACCOUNT_ID>` with the actual account ID and adjusting regions.

---

### **Additional Configuration Examples**

You can refer to the official [AWS Nuke GitHub repository](https://github.com/rebuy-de/aws-nuke) for more configuration examples and detailed documentation.

--- 

## **Conclusion**

AWS Nuke is a powerful tool for cleaning up AWS resources in a safe and automated manner. While it's extremely useful for cleaning up dev/test environments, **always ensure you're using AWS Nuke in non-production environments** to avoid accidental data loss or service disruption.
