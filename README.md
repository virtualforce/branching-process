# Repo Management - Branching Approach

Javed Zahoor, Virtual Force

This document highlights the branching methodology used at Virtual Force to meet our highly dynamic and collaborative working needs. The process is pretty standard and adopted to work for our diverse tech teams. 

In this article, I do not intend to cover the basics of how to use a repository. If you are looking to understand the basics of Repository, Branching or Merging etc. you may want to take at look at [GitHub's Hello World Guide](https://guides.github.com/activities/hello-world/)

At Virtual Force we are open to all kinds of tech stacks includding but not limited to PHP, ROR, Java, Android, iOS, Hybrid and Web development using [Appery](https://appery.io/), [Meteor](https://www.meteor.com/), [MEAN](http://mean.io/), [Stamplay](https://stamplay.com/), etc. and meshups; and we dont mind mixing and matching the technologies to the best of our interest. 

Ofcourse with so diverse technology components at hand, it takes some level of energy and thinking to plan-out a unified process that can work with all our teams. Today I will be discussing Branching process we use at Virtual Force.

For every new project we create a separate repository which contains the following branches

## `main` branch
Every repository has one `main` branch which is supposed to contain the most *stable* and *logically complete* version of the code which is tagged as v1.0, v2.0 etc. As a standard process we use the following branches in our code base. Any person joining the team can pull this code to configure the application and start with their *orientation process*.

### `production` branch
Every repo has one `production` branch which represents the code EXACTLY as found in the production environment. If there is a need to reseruct a production environment from the repository (ideally it should be done from  an image of the production server), this branch is going to be deployed in the production, after preparing the base server as per *Server Configuration Guide* for each system, prepared and maintained each our [team collaboration tool](http://www.teamwork.com).

As soon as the code is committed to this branch, our CI *(Continous Integration)* setup, if configured, triggers a deployment for the production. 

### `hotfixes` branch
In an agile-*ish* company like ours, it is pretty normal to find ourselves in a situation where we need to release a hostfix for production quickly, regardless of the state of the dev branch. Thus, if and when needed, we create a `hotfixes` branch from `production`, troubleshoot and fix the problems with the code and *up-merge* the code back to `production`.

At the same time, the changes from `hotfixes` branch is also *down-merged* to the `dev` branch so the next wave of changes coming from dev doesnt miss the hotfix or its ripples.

After the branch has been merged in both the `production` as well as the `dev` branches, the hotfixes branch is deleted to cleanup the repo.

### `staging` branch
The purpose of this branch is to simulate a `dev` deployment into `production`. It is mainly used to perform regression before final deployment. Thus the branch originates from `production` and is kept in synch before starting to test a new *candidate release*. The *candidate release* is pushed to stating, tested and fixed for any defects on this branch before eventually being pushed to the `production` branch. 

### `development` branch
This branch is for the dev members to merge all *feature branches* a.k.a *Integration testing* before pushing the code from `dev` to `staging`.

### `<feature>` branch
For every new feature, a new branc is created bearing the feature name as branch name. This helps in quickly identifying what to expect in that branch of the code. This is the branch where the dev staff is usually making all the major code changes and adding test scripts for the new feature being developed. 

Most of our projects stop at this level of branching depth, however in some cases, where one feature is subdivided into multiple relatively independant parts such that different developers can work on each part separately, this is managed by *optionally* creating sub-branches from the `<feature>` branch as <FL-feature> where *FL* are initials of the concerned developer working on this branch. But this comes with an extra overhead of having to *up-merge* these back to `<feature>` before being finally able to test the feature.

#### Rebasing a breanch

If a feature branch gets pretty old (more than 2 branches old from the dev branch), it may start posing *up-merging* challenges, thus should be *rebased* to the newest `dev` branch.


## The Process
Here is the process at a glance

![Branching Scheme](https://github.com/virtualforce/branching-process/images/branching.png "Branching Scheme")


Event | Source Branch | Destination Branch | Responsible Person | Steps
--- | --- | --- | ---| ---
New Project | | `main` | Project Owner | 1. Create a `<Project>` repository <br/> 2. Create `production`, `staging` and `development` branches <br/> 3. Add project contributors with appropriate rights
Sprint Start | `development` | `<feature>` | Project Manager | 1. Create `development` -> `<feature>` branch <br/> 2. (Optional) Create `<feature>` -> `<FL-feature>`
(Optional) Feature-Part Complete | `<FL-feature>` | `<feature>` | Developer | 1. up-merge `<FL-feature>` -> `<feature>` branch
Feature Complete | `<feature>` | `development` | Developer | 1. up-merge `<feature>` -> `development` branch
Candidate Release Ready | `development` | `staging` | Project Manager / Team Lead | 1. up-merge `development` -> `staging` branch <br/> 2. if CI is not enabled, manually deploy from `staging` to QA server
Release Ready | `staging` | `production` | Project Manager | 1. up-merge `staging` -> `production` branch
Project Closure | `production` | `main` | Project Manager | 1. up-merge `production` -> `main` branch <br/> 2. Tag/label the main branch
Hot Fix Required | `production` | `hotfixes` | Project Manager | 1. Create `hotfixes` <- `production`
Hot Fix Ready | `hotfixes` | `production` | Project Manager | 1. up-merge `hotfixes` -> `production` <br/> 2. down-merge `hotfixes` -> `development`

