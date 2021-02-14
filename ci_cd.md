## Git Branching Policies
* Commit intial code in main/master
* Create a sub-branch develop
* Create all sub-stories in develop subbranches(Feature Branch)
* Once coding is complete, commit the code in Feature Branch and raise Pull Request(PR) from feature to develop

## Automated Pull Request(PR) Review Setup(Branch Policies for develop)
* Minimum number of reviewers set
* If required, requestors allowed to approve their own changes
* Check for Comment Resolution kept as Required
* Limit merge types can be disabled. If need be, it can be kept as any type(Basic Merge-Stores commits for all, Squash Merge-Gives only one commit and keeps develop commits clean, Rebase and fast forward, Rebase with merge commit). Mostly developers prefer basic merge or squash merge
* Automatic trigger for Build Validation kept for prior validation of CI Build stability. It makes a temporary copy of develop and appends the feature commit and build the code
* Optional reviewers can be kept

## CI Build for PR for Feature Branch
* Source and destination branches chosen
* Maven Authenticate used for common library JARs(Kept as feeds in artifacts)
* Maven : Used for goal package, Publishing JUnit Test Results to Azure Pipelines, Test result files for Surefire XMLs given
* Then it is saved
* Once queued, it starts agent job where a new OS instance(Eg. Ubuntu) given for each agent job run
* Once job run succeeds, feature branch gets merged to develop branch
* Post develop merge, CI/CD Build initiates

