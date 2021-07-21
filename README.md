﻿# AWS Developer Associate Notes
 
 ## IAM
 
 Identity and Access Management
 Manage users and their level of access to the console
 
 Centralized control over account
 Shared access to AWS account
 Granular Permissions
 Idenetity Federation (use other creds like fb, linkedin, etc)
 
 1. Multi-factor Authentication
 2. Temporary Access for uses.devices/services
 3. Password Policies
 4. Integrate with AWS Service
 5. Compliance

Users : End Users, people
Groups: Collection of users under one set of permissions
Roles: Define set of permissions and can be assigned/assumed by users or services like ec2 
Policy: Document that defines one or more permissions (can be attached to any of the above)

Testing IAM permissions
- IAM Policy Simulator
- test the effect before commiting
- validate its working as expected
- test policies already attached to users (great for troubleshooting)

Universal not regional

Root account is account you created when you created the AWS account (don't use)

New users have 0 permissions when first created.

New users are assigned access keys.secret acces keys (not the same as a password to login) used for progammatic access
Can only see once when the user is created (SAVE)
recommended MFA
Customize password rotation policies



