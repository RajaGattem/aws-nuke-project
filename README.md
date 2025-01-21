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
- [License](#license)

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

### Explanation of the Command:
- `sed`: The stream editor for performing text transformations.
- `-i`: Edits the file **in place**, modifying the original file directly.
- `'s/<ACCOUNT_ID>/123456789012/g'`: The substitution expression:
  - `s`: Stands for **substitute**.
  - `<ACCOUNT_ID>`: The text you want to replace.
  - `123456789012`: The account number that will replace `<ACCOUNT_ID>`.
  - `g`: **Globally** replace all occurrences in the file (not just the first one).
- `config.yaml`: The name of the file where the substitution will take place.


### 2. **Verify the Changes**:
After running the `sed` command, verify that the replacement has been made:

```bash
cat config.yaml
```

This will print the contents of the file, where you should see `<ACCOUNT_ID>` replaced with `123456789012`.

### Summary:
- **Replace `<ACCOUNT_ID>` with a  account number**: Use `sed -i 's/<ACCOUNT_ID>/123456789012/g' config.yaml`.
- **Backup before modification**: Use `sed -i.bak` to create a backup file (`config.yaml.bak`).
- **Verify changes**: Use `cat config.yaml` to confirm the changes.

Let me know if you need further assistance!
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
2. Verify that your `config.yaml` file is correctly configured.
3. Run the AWS Nuke command to delete resources as described in the **Usage** section.

---

## **Safety Considerations**

- **Critical Operation**: AWS Nuke **deletes all resources** in the specified AWS account and regions. Use it with extreme caution.
- **Dry-Run Mode**: Always use the `--no-dry-run` option first to verify which resources will be deleted before executing the command.
- **Backup**: Ensure that any critical data (such as S3 buckets or RDS databases) is backed up before running AWS Nuke.

---

## **Contributing**

AWS Nuke is an open-source project, and contributions are welcome! If you find bugs, want to add features, or make improvements, please follow these steps:

1. Fork the repository.
2. Create a new branch for your changes.
3. Implement the changes and write tests if necessary.
4. Submit a pull request with a description of your changes.

For more detailed contribution guidelines, check out the repository’s [CONTRIBUTING.md](CONTRIBUTING.md) file.

---

## **License**

AWS Nuke is licensed under the **MIT License**. See [LICENSE](LICENSE) for more details.

---

## **Additional Resources**

- AWS Nuke GitHub Repository: [https://github.com/rebuy-de/aws-nuke](https://github.com/rebuy-de/aws-nuke)
- AWS CLI Documentation: [https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

---

