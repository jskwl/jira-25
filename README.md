# Jira-Git connection

## Phase 1 : Project Setup and Initial Data
- Jira Settings > System
- External system import
- Switch to old experience
- csv
- select file : issue-sample.csv
- Import Project : Select your project
    - Date format set to dd/MM/yyyy
  - Map CSV Field to JIRA field and check Map field value
    - done date = Resolution
    - story points = Story point estimate
  - Begin import (See preview)
  - Set Sprint date from 24/10/2025 to 07/11/2025

### Smart Commits
- The user email for the git commit message must match what is present in Jira
- the correct format for a commit command is
```sh
git commit -am "EP-6 #done #comment Completed via Smart Commit"
git commit -am "EP-3 #comment Entire process for updating Jira #to-do #time 30m"
```
- Not the EP-6, should match the Jira Id
- transition must be present in the workflow eg. #done or #to-do
- Comment using #comment <comment message>
- To see time added with #time, You must enable it in the Space-Settings Work Type : Bug/Story/Task

### Setting up Github or GitLab
- Install Github for Atlassian
  -	Apps click : Explore more apps
  -	Search: GitLab for Jira Cloud by GitLab
  -	Or 
  -	Search: GitHub for Atlassian by Atlassian Free
-	Configure/Un-Install Github-Jira-application
  -	App Switcher  
    -	Administration
    -	Apps > Sites> <Name_of_site>
  -	Connected Apps
  -	Github for Atlassian : Uninstall / GetStarted
  - When commits are made to repo email must match Jira user name
    - git config user.email
    - git config user.email "your@email.com"

### In Jira Manual update
- set due date of 3 tasks to 3 weeks ago to make them stale
- Use the above for exeception reporting

---

## Phase 2 : Advanced Reporting and Dashboard Creation

### JQL Filter (Get Stale report)
- Jira > Filters (Bottom of left side bar) / (type filters in search and select filter)
- Create Filter - top right
- IN JQL use following query
```JQL
project = ech-projects AND assignee = currentUser() and status != Done AND due< now()

project = ech-projects and assignee IS EMPTY
```

### Dashboard
- Add a Gadget and search the following
  - Pie chart 1
    - Project or Search Filter: ech-projects
    - Statistic Type : Status
  - Pie chart 2
    - Project or Search Filter: ech-projects
    - Statistic Type : Assignee
  - Filter Results
    - All tasks that are Due and incomplete (Saved Filter)
    - Num resluts : 10
    - Cols to display:
      - Issue Type / Key / Summary
      - Priority / Assignee
- To rename the titles
  - Double click the visuals and rename
  - Click Done to complete
- Rearrange view

----
## Phase 3: Automation and Process Improvement

### Step 1: Create an Automation Rule for Stale Issue Management
- Goto Project > Settings
- Click on Automation
- Create Rule
- When Trigger
  ```
  # good
  project=ech-projects AND assignee=currentUser() AND due<now() and status != Done

  # extra values
  updated < "-7d"
  ```
- + Add Components
  - Add a branch
  - Branch rule / related work items
    - For Current Work Item
  - Then: Send Email
    - To: Assignee (From drop down)
    - Subject: Action required: {{issue.assignee.emailAddress}}
    - Content :
    ```
    Hi {{issue.assignee.displayName}},

    Issue with: {{issue.key}}
    Issue: {{issue.summary}}
    Review url: {{issue.baseUrl}}
    ```
    - add Comment to work item
    - Validate rule


### Step 2: Create an Automation Rule for Work Logging
- Create rule
- Devops > Commit created - Next
- Then Transition work item : In progress
- Name rule: Set Transition on Commit
- Validate Rule
- Test using : git push
  ```
  git commit -am "EP-3 #comment Auto change progress #time 30m"
  ```


### Step 3: Implement Workflow Screen Required Fields

### Step 4: Final Review and Documentation

-----

## Jira - Jenkins - How To

### Jira Cloud API token (typical for basic REST auth)

1. Sign in to the Atlassian account that will be used by Jenkins (or create a service account).

2. Go to Manage API tokens (your Atlassian account) → Create API token → copy the token (it won’t be shown again). 
Atlassian Support
  - To get Key from Jira : 
    - User Icon > Account Settings > Security > 
      - Create and manage API tokens > Create API token

3. In Jenkins, go to Credentials → (choose scope) → Add Credentials and choose Secret text (paste the API token). Jenkins treats this as a secret/token. 

4. Configure the Jira plugin or Jenkins job to use that credential (select the credentials entry you created).

-----
