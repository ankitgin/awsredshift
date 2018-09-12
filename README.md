# Redshift Encryptor

This script will help to easily encrypt an unencrypted Amazon Redshift cluster. As part of the script, this script will copy your database from an unencrypted cluster Amazon Redshift to an encrypted Amazon Redshift cluster. You will need to have a pre-configured Amazon Redshift cluster with encryption enabled before executing the script. You can also use the SNS service to monitor the progress of the script.

## Notes

1. The destination Encrypted Amazon Redshift cluster should be of higher or same configuration compared to the source Amazon Redshift unencrypted cluster.
2. Please note, Only one database can be migrated at a time.For multiple Databases, you need to run the script multiple times.
3. You will need to create an s3 bucket which will be used to unload and copy the cluster data and the S3 bucket should be in the same region where the source Amazon Redshift cluster exists.
4. Passwords cannot be migrated and hence must be reset once the migration is completed.
5. We can not create duplicate users and groups, so before migrating the common users in the destination cluster will be deleted.
6. Please supply valid input details when prompted for or it may break migration.
7. Make sure no user in source cluster except master user has the same name as a master user of the destination cluster.
8. Master user from source will not be copied to the destination as the destination will have its own master user. Also, superuser will not be migrated.
9. Historic information that is stored in STL and SVL tables is not migrated or retained in the new cluster.
10. Amazon S3 log settings are not migrated, so be sure to enable database audit logging on the new cluster.

## AWS Services involved in the script

1. AWS Redshift
2. AWS IAM
3. AWS S3
4. AWS SNS (Optional)

As resources of these services will be spun, there might be some additional charges for using these services. Please refer the documentation [1] for details about the charges.

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
$ git clone https://github.com/#####PATH########
```

3. Change directory to amazon-redshift-utils/src/#####PATH########/ and run the script “migration.py” using python3.
```sh
$ python3 migration.py
```
Please also make sure you make the cluster read only during the process i.e. cut all the write inputs.
Please follow the instructions in the script.


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

## References 
[1] https://aws.amazon.com/pricing/services/ 
