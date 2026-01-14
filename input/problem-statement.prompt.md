## Background

1. We are a small-medium companies where we have QA:Dev ratio of 1:10
2. We are mostly striving for shipping new features & fixing/hot-fixing bugs fast.
3. We build & maintain in house SAAS procurement platform called Adam (https://adam-procure.com/).
4. So far, we have 3 application namely Adam, Hub & Eva that are interconnected with each other.

### Current Git Workflow

Below is our standard branches (2 of them are used to deploy to an environment):

1. `master` branch
    - This is the branch the represents the current `staging` environment code
    - Upon code merge into this branch, GitHub Action will trigger a deployment to `staging` environment
2. `production` branch
    - This is the branch that represents the current `production` environment code
    - Upon code merge into this branch, GitHub Action will trigger a deployment to `production` environment
3. `ST-XXX/FEAT/<name>` branch
   - This branch are meant for feature development
   - This branch is not tied to any deployment environment
4. `ST-XXX/FIX/<name>` branch
   - This branch are meant for bugfixes
   - `This branch is not tied to any deployment environment
5. `ST-XXX/DEPLOYMENT-<date>` branch
   - This branch are meant to be the pre-production branch that represents the next sets of changes in `production` environment during **FULL DEPLOYMENT** flow
   - This branch is not tied to any deployment environment
5. `ST-XXX/CHERRY-PICK/<date>` branch
   - This branch are meant to be the pre-production branch that represents the next sets of changes in `production` environment during **CHERRY-PICK DEPLOYMENT** flow
   - This branch is not tied to any deployment environment

We have 2 deployment flows:

#### Full Deployment Flow

1. Developer will branch out a new feature (`ST-XXX/FEAT/<name>`) or bugfix (`ST-XXX/FIX/<name>`) branch from `master` branch.
2. Once development is done, the Devs will create PRs to merge their feature/bugfix branch into `master` branch
3. Then, QA will trigger the PR closing once it has been reviewed. This action will merge the PR changes into `master` branch & GitHub Action will trigger deployment to `staging` environment
4. Once everything passes QA & ready for deployment, Devs will create a new full deployment (`ST-XXX/DEPLOYMENT-<date>`) branch from the `production` branch to prepare for production
5. The Devs responsible for deployment that week will merge `master` branch's changes into the deployment branch. During this time, there might be a significant amount of merge conflicts to be resolved.
6. Then, they will create a new PR to merge the deployment branch into `production` branch.
7. Once its merged, GitHub Actuion will auto deploy the changes to `production` environment.

#### Cherry-Pick Deployment Flow

1. Developer will branch out a new feature (`ST-XXX/FEAT/<name>`) or bugfix (`ST-XXX/FIX/<name>`) branch from `master` branch.
2. Once development is done, the Devs will create PRs to merge their feature/bugfix branch into `master` branch
3. Then, QA will trigger the PR closing once it has been reviewed. This action will merge the PR changes into `master` branch & GitHub Action will trigger deployment to `staging` environment
4. Once everything passes QA & ready for deployment, Devs will create a new cherry-pick deployment (`ST-XXX/CHERRY-PICK/<date>`) branch from the `production` branch to prepare for production
5. The Devs responsible for deployment that week will merge do `git cherry-pick` from `master` branch into the deployment branch. 
6. Then, they will create a new PR to merge the deployment branch into `production` branch.
7. Once its merged, GitHub Actuion will auto deploy the changes to `production` environment.

## Our Challenges

### Developer's pain points

- We need to ship new features as fast as possible since delaying a feature might result in our customer seeking other procurement solutions that have said features ready in place for them
- We also need to fix/hotfix any bug on the spot as fast as possible since it might reflect our credibilities
- We have issue when juggling between code review & actual development since everyone are busy with development proccess

### QA's pain points

- Code Review process is their bottleneck because they need to wait for code review process to finish before they can proceed with their testing
- They are involved in a project quite late of the development lifecycle

## Objective

1. List the popular git-flow that is implemented by other successful SAAS enterprise.
2. Explain what challenges they are facing & their Git Flow helped their Tech team excel in managing their business & customer's expectation as well.
3. Explain why it work for their specific case.
4. You should also explain how, where and when they apply `git merge`, `git rebase`, `git cherry-pick` & other git related feature and their best practices.


## Output

1. Output your findings in `output/git-workflow-rnd.md`.
2. Your result should be written preferably in Markdown formatted documents.
3. List out the pros & cons for each approaches in tabular format. In the conclusion section, summarize of all your findings in tabular format
4. Add in visual diagram wherever possible to better explain the workflow & make sure to those diagrams HAVE SIMILAR FORMAT so that the content are consistent.
5. Based on your findings, you should suggest what are the best practices that we can employ for our specific problem such where, when & how do we use `git merge`, `git rebase`, `git cherry-pick` & other git related feature and their best practices.
6. Make it easy to understand for both senior software engineer & junior devops engineer.
