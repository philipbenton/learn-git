Branches

Branches are key to a successful git workflow and should be used correctly throughout development & deployment process.

It is extremely important to branch, commit and merge often and to not get lazy or complacent with it.

Ensure commit messages are detailed enough to ensure a new developer can understand the commit.

Deployment Branches

Let’s talk about deployment branches… We will have a branch for each deployment environment making it easier and safer to deploy to each environment. Deployment is handled by Beanstalk who support multiple environments and servers.

The environment branches we use are:


   * Production
   * Stage
   * …

By default all environments will be set to manual deployment within the Beanstalk interface, meaning Beanstalk will NOT deploy to the server when changes are committed to the repository. This can be changed if needs be, however, under no circumstance should the production environment be set to automatic deployments, an added layer of security.

For convenience Beanstalk do provide commands which can be used within commit messages/comments to run a deployment on commit without having to manually initiate the deployment through the Beanstalk interface.

Commands look like this - [deploy: ENVIRONMENT], where environment is the environment you wish to deploy to:

[deploy: stage]

Development Branches

There are 2 types of development branches we will use; feature or topic branches and bug fix branches.

Feature or Topic Branches

Feature or Topic branches are where we spend the majority of our time. These branches will come from our Master/Stable branch and are used to develop new features.

Features or Topic examples include:


   * Adding a footer to a page
   * Developing particular functionality

Bug Fix Branches

Bug Fix branches are used when a fix is required on a feature or topic which has previously been completed and merged back into the Master/Stable and Environment branches.

The workflow for this is where it gets slightly complicated. The best way to understand this is to read a great entry from the book Pro Git here:

http://git-scm.com/book/en/Git-Branching-Basic-Branching-and-Merging


Merging Branches

All features/topics and bug fixes should be merged back into the Master/Stable branch once complete before being merged into the Production environment branch.

The basic workflow is as follows:


  1. Create new feature branch from Master/Stable
  2. Develop the new feature
  3. Commit new feature
  4. Merge the new feature to Stage environment
  5. Deploy to Stage for testing
  6. Once testing is complete merge the feature branch to Master/Stable
  7. Note: Master/Stable should now be at the same commit as Stage
  8. Merge Master/Stable into Production
  9. Deploy to Production

Beanstalk have an excellent article on deployment & branches here:

http://guides.beanstalkapp.com/version-control/branching-best-practices.html


From the article...
Never Merge to Environment Branches Without Deploying
In order to keep your environment branches in sync with the environments, it’s a best practice to only execute a merge into an environment branch at the time you intend to deploy. If you complete a merge without deploying, your environment branch will be out of sync with your actual production environment.
With environment branches, you never want to commit changes directly to the branch. By only merging code from your stable branch into staging or production branches, you ensure that changes are flowing in one direction: from feature and bug-fix branches to stable and staging environments, and from stable to production. This keeps your history clean, and again, lets you be confident about knowing what code is running in which environments.
Stage 1. Developing a new feature.Stage 2. Feature is completed and tested.Stage 3. Feature is being released.


A note on merge conflicts:

Merge conflicts occur when a file being merged has been changed differently on the 2 branches being merged (the simple, plain english version). To resolve this we use Kaleidoscope App.


Tagging

Tagging is another important aspect of our git workflow. Basically every time we deploy to production we should be first tagging the release.

We should version tags as per this note: Software / Theme Version Numbering

Only create Tags from the Production branch once the successful deployment has been made.

Basically:

   * The first production release will be 1.0.0
   * Bug fixes & WP Core / Plugin Updates will increment the 3rd digit, i.e 1.0.1, 1.0.2, 1.0.3, etc
   * Minor releases, i.e new features will increment the 2nd digit, i.e 1.2.0
   * Major releases will increment the 1st digit, i.e 2.0.0. Major releases are likely to be a complete theme redesign (this is a WordPress workflow)

Note:

Do not confuse tag version numbers with branches. For example if we release feature X as tag 1.1.0 (as an example) and then begin release feature Y as tag 1.2.0, but then feature X has a but which needs fixing the tag does not resort back to 1.1.0. The release tag will be 1.2.1 (minor bug fix, regardless of which feature needed the fix).

However, when branching we would checkout a new branch from the branch at which feature X was released, make the bug fix, test and merge back into the Master/Stable. Then merge to Production and deploy.


Typical Workflow


  1. Checkout new branch from Master/Stage - new-feature-a
  2. Work on the new feature
  3. Commit changes
  4. Marge into Stage branch
  5. Deploy Stage
  6. Test the feature in the stage environment
  7. Oops, found a problem...
  8. Checkout new-feature-a branch
  9. Continue work
  10. Commit changes
  11. Merge into Stage branch
  12. Deploy Stage
  13. Test the feature in the stage environment
  14. Great, let’s release the new feature
  15. Checkout Master/Stable
  16. Merge in new-feature-a
  17. Checkout Production
  18. Merge in Master/Stable
  19. Deploy Production
  20. Create Tag if required
  21. Delete branch new-feature-a
  22. Master/Stable, Stage, and Production, Tag X.X.X should now all be at the same commit

Oops, we’ve found a bug in a previous feature release


  1. Checkout new branch from Master/Stable
  2. Fix the bug
  3. Commit changes
  4. Merge into Stage branch
  5. Deploy Stage
  6. Test the bug fix in the stage environment
  7. Great, fix worked
  8. Checkout Master/Stage
  9. Merge in bug fix branch
  10. Checkout Production
  11. Merge in Master/Stable
  12. Deploy Production
  13. Create Tag if required
  14. Delete bug fix branch

Note: If we were working on a new feature in a separate branch before fixing the bug and now want to continue working on the feature we will checkout the feature branch. However, we may want to merge in the Master/Stable again which will contain the bug fix. This will be useful if we update the WP Core or Plugins not just fix bugs.