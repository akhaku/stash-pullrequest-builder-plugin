Stash Pull Request Builder Plugin
================================

[![Join the chat at https://gitter.im/nemccarthy/stash-pullrequest-builder-plugin](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/nemccarthy/stash-pullrequest-builder-plugin?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This Jenkins plugin builds pull requests from a Atlassian Stash server and will report the test results as a comment.
This plugin was inspired by the GitHub & BitBucket pull request builder plugins.

- Official [Jenkins Plugin Page](https://wiki.jenkins-ci.org/display/JENKINS/Stash+pullrequest+builder+plugin); https://wiki.jenkins-ci.org/display/JENKINS/Stash+pullrequest+builder+plugin
- See this [blogpost](http://blog.nemccarthy.me/?p=387) for more details; http://blog.nemccarthy.me/?p=387 


##Prerequisites

- Jenkins 1.532 or higher.
- Git Plugin - https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin


##Creating a Job

- Create a new job
- Select Git SCM
- Add Repository URL as bellow
  - git@myStashHost.com:${projectCode}/${repositoryName}.git
- In Branch Specifier, type as bellow
  - */${sourceBranch}
- Under Build Triggers, check Stash Pull Request Builder
- In Cron, enter crontab for this job.
  - e.g. every minute: * * * * *
- In Stash BasicAuth Username - Stash username like jenkins-buildbot
- In Stash BasicAuth Password - Jenkins Build Bot password
- Supply project code (this is the abbreviated project code, e.g. PRJ)
- Supply Repository Name (e.g. myRepo)
- Save to preserve your changes

##Merge the Pull Request's Source Branch into the Target Branch Before Building

You may want Jenkins to attempt to merge your PR before doing the build -- this way it will find conflicts for you automatically.

- Follow the steps above in "Creating a Job"
- In the "Source Code Management" > "Git" > "Additional Behaviors" section, click "Add" > "Merge Before Building"
- In "Name of Repository" put "origin" (or, if not using default name, use your remote repository's name. Note: unlike in the main part of the Git Repository config, you cannot leave this item blank for "default".)
- In "Branch to merge to" put "${targetBranch}" 
- Note that as long as you don't push these changes to your remote repository, the merge only happens in your local repository.


If you are merging into your target branch, you might want Jenkins to do a new build of the Pull Request when the target branch changes.
- There is a checkbox that says, "Rebuild if destination branch changes?" which enables this check.

##Notify Stash Instance (StashNotifier plugin)

If you have enabled the 'Notify Stash Instance' Post-build Action and also enabled 'Merge before build', you need to set '${sourceCommitHash}' as Commit SHA-1.  This will record the build result against the source commit.

##Rerun test builds

If you want to rerun pull request test, write *"test this please"* comment to your pull request.

##Adding additional parameters to a build

If you want to add additional parameters to the triggered build, add comments using the pattern `p:<parameter_name>=<value>`, one at each line, prefixed with `p:`. If the same parameter name appears multiple times the latest comment with that parameter will decide the value.

**Example:**

    test this please
    p:country=USA
    p:env=dev1


## Post Build Comment

It is possible to add a post build action that gives the option to post additional information to Stash when a build has been either successful or failed.
These comments can contain environment variables that will be translated when posted to Stash.

This feature can be used to post for instance a url to the deployed application or code coverage at a successful build and why the build failed like what tests that did not pass.

##Copyright

Copyright © 2015 Nathan McCarthy.


##License

- BSD License
