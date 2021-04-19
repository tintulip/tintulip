# Bootstrap owners

Below are the steps we took from the initial aws account creation, to creating individual IAM admin users. 


- Temporary user was created manually by root account with aws managed administrator access policy attached directly to the temporary user.
- Temporary user generated access and secret keys to use locally for the terraform code. 
- Temporary user applied terraform code and pushed the local state back up to the repository.
- terrafom.tfstate contains encrypted initial passwords for the IAM users and was pushed.
- Steps taken to set up IAM user accounts in [bootstrap users repo read me](https://github.com/tintulip/bootstrap-users/blob/main/README.md).
- Individual IAM users decrypted their own passwords, set up MFA and changed passwords.
- Temporary user is now deleted.