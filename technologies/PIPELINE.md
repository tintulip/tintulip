# Using Github Actions to set up CI/CD 

## Prerequisites

In order to set up the CI/CD pipeline there was `four components` that was needed:
    
1. S3 Bucket for storing terraform state.
2. IAM user with programmatic access.
3. IAM role for the IAM user to assume.
4. Defining Github Secrets with IAM user access and secret Keys.

These were created manually via the Amazon console. 

## Defining the pipeline
To ensure that only members of team dali are able to trigger the pipeline, in the settings of the repository in the `Branches` section there is `Restrict who can push to matching branches` and editied to allow specific memebers to push.

When a commit is pushed to the main branch the pipeline will run a series of steps. The access and secret keys are pulled from `GitHub secrets`and placed within environment variables. 

The selected actions match certain specified criteria which are defined in the settings, which are actions created by GitHub and actions in the marketplace created by verified creators.The action permissions are set under the actions section in the specified repository settings.    


## Future Considerations
Access and Secrets keys are static and need to be rotated every 90 days.