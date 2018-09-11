# On Demand Redshift

This script will help you to encrypt your unencrpted cluster based on your requirement. As part of the script, this script will copy your database from a unencrpted cluster to encrpted cluster. You will need to have a fresh encrpted cluster before executing the script. You can also use the SNS service to monitor the progress of the script.


## Setting the environment

1. Please run the following commands in terminal to install python3 and the necessary dependencies in order to run the script:
```sh
$ sudo yum install python36 postgresql postgresql-devel gcc python36-devel libffi-devel
$ curl -O https://bootstrap.pypa.io/get-pip.py
$ python3 get-pip.py --user
$ pip3.6 install PyGreSQL boto3 pytz --user
```

2. Clone the repository using the following command:
```sh
git clone https://github.com/#####PATH########
```

3. Change directory to amazon-redshift-utils/src/#####PATH########/ and run the script “migration.py” using python3.
```sh
python3 migration.py
```

Please follow the instructions in the script.


## Customer Notes

1. Customer must have a source unencrypted cluster and destination Encrypted cluster with higher or same configuration.
2. Customers can only migrate one database at a time 
3. A Customer must create an s3 bucket which will be used to unload and copy cluster data.
4. Please note that passwords cannot be migrated and hence must be reset once the migration is completed.
5. Please follow the instructions displayed.
6. Please enter proper details when prompted for or it may break migration
7. Please provide an S3 bucket for unload and copy commands, the S3 bucket should be in the same region as the source.
8. make sure your CLI is configured.
9. make sure you have installed python 3+ and boto3 installed.

## Limitations
1.Use new cluster as a destination cluster is recommended to avoid namespace clashes which may result in migration failure.

## Notes
1. Master user from source will not be copied to the destination as the destination will have its own master user.
2. We will be creating and deleting a role for this process which will have s3 access, so please do not modify the role while the process is running.
3. Amazon Simple Storage Service (Amazon S3) log settings are not migrated, so be sure to enable database audit logging on the new cluster.
Historic information that is stored in STL and SVL tables is not migrated to or retained in the new cluster.

## Point to Remember

1. ALWAYS REMEMBER THAT WE CANNOT CREATE DUPLICATE USERS AND GROUPS, So before migrating the common users in destination cluster will be deleted.
2. It is a best practice to make sure the destination cluster is new and empty.
3. And also note that superuser will not be migrated.
4. Make sure no user in source cluster except master user has the same name as a master user of the destination cluster.


Please also make sure you make the cluster read only during the process i.e. cut all the write inputs.

## Running the script

### 1. Select your Amazon Redshift cluster, and choose Manage IAM Roles.
Note all IAM roles that are associated with your cluster and save the roles to be used in the new cluster.
Choose the Details pane of your cluster, and note the following configurations:
- Node Type
- VPC ID
- VPC security groups
- Cluster Parameter Group
- Enhanced VPC Routing
- Cluster Database Properties
- Port
- Publicly accessible
 
### 2. Post Restore
At the time of restore, Make sure the following properties are the same compared to the source.
- Node Type
- Cluster Identifier
- Port
- Cluster subnet group
- Publicly accessible
- Enhanced VPC Routing
- Cluster Parameter Group
- VPC security groups.
 
### 3. Associate IAM roles
- Open the Amazon Redshift console, and choose Clusters from the navigation pane.
- Select the new cluster and choose Manage IAM Roles.
- From the Available roles, choose the roles associated with the source cluster.
- Choose Apply changes.
