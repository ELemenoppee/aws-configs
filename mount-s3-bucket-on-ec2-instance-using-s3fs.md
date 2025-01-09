# Mount S3 Bucket on EC2 Instances using S3FS

## Overview

Amazon S3 (Simple Storage Service) is a scalable cloud storage solution that allows you to store and retrieve data efficiently. To interact with your S3 bucket directly from a Linux environment, such as CentOS, RHEL, or Ubuntu, you can mount the S3 bucket as a file system using the s3fs-fuse tool. This enables seamless access to the bucket as if it were a local directory.

## Prerequisites

Before proceeding, ensure you have the following:

+ An active Amazon AWS account.
+ An S3 bucket created in your AWS account, along with your Access Key ID and Secret Access Key.
+ The S3 bucket must be set to private to enhance security.

## Steps

### 1- Installing s3fs-fuse

Begin by installing the s3fs-fuse package, which is required to mount an S3 bucket. Run the following commands in your terminal:

```bash
sudo apt-get update && sudo apt-get install s3fs
```

This command ensures your system is up-to-date and installs all necessary dependencies for s3fs.

### 2- Configuring AWS Credentials

To securely provide your AWS credentials, create a hidden file named .passwd-s3fs in your home directory. Store your credentials in the following format:

```bash
echo ACCESS_KEY_ID:SECRET_ACCESS_KEY > ${HOME}/.passwd-s3fs
chmod 600 ${HOME}/.passwd-s3fs
```

Replace ACCESS_KEY_ID and SECRET_ACCESS_KEY with your actual credentials. Setting the file permissions to 600 ensures that only the owner can access it, enhancing security.

### 3- Creating a Mount Point

Decide where you want to mount your S3 bucket. Create a directory to serve as the mount point:

```bash
mkdir /path/to/local/mountpoint
```

For example, you might create /home/ubuntu/s3-mount. Afterward, modify the permissions to allow access:

Modify the permissions for the mount directory:

```bash
chmod 777 /path/to/local/mountpoint
```

### 4- Mounting the S3 Bucket

Use the following command to mount your S3 bucket to the previously created directory:

```bash
s3fs mybucketname /path/to/local/mountpoint -o passwd_file=${HOME}/.passwd-s3fs
```

Replace mybucketname with the name of your S3 bucket and /path/to/local/mountpoint with the directory path you created.

To confirm the successful mounting, check the disk usage:

```bash
df -h
```

### 5- Testing and Verification

To verify the setup, upload a file (e.g., an image) into the mounted directory. Then, check your S3 bucket via the AWS Management Console to confirm that the file appears there.

This ensures the local directory is correctly linked to your S3 bucket.

### 6- Ensuring Persistent Mounting

To make the mount persistent across reboots, add an entry to the /etc/fstab file:

```bash
echo 's3fs#mybucketname /path/to/local/mountpoint fuse _netdev,allow_other 0 0' | sudo tee -a /etc/fstab
```

This ensures the bucket is automatically remounted whenever the system restarts.

## Summary

By following these steps, you can efficiently mount an Amazon S3 bucket on your Linux-based EC2 instance. This setup enables direct interaction with your S3 storage, providing a seamless experience similar to working with local files while maintaining the flexibility of cloud storage.