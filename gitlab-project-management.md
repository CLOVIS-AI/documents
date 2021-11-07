# Managing projects with GitLab

This document takes inspiration from:

- [GitLab code review guidelines](https://docs.gitlab.com/ee/development/code_review.html)
- [GitLab value stream documentation](https://about.gitlab.com/solutions/value-stream-management/)
- [GitLab issue triage](https://about.gitlab.com/handbook/engineering/quality/issue-triage/)

## Issue tracking

This document presents a conventions to organize projects. Following these conventions allow for easier project management.

### DevOps

The development process looks like the following stages:

- Issue: A task has been created, but it hasn't been scheduled yet (added to a board or a milestone)
- Plan: A task has been scheduled, but no one has started working on it yet (stops when the issue is mentioned in a pushed commit)
- Code: Someone is currently working on it (starts when the first issue is mentioned in a commit, stops when a non-draft merge request is created)
- Test: The CI pipeline is checking the code
- Review: The reviewers are checking the code
- Staging: The CD pipeline is deploying the code

More information can be found in the [GitLab documentation](https://docs.gitlab.com/ee/user/analytics/value_stream_analytics.html), in particular [details about the measurement of each stage](https://docs.gitlab.com/ee/user/analytics/value_stream_analytics.html).

### Labels

You should create labels for the different subprojects (eg. `server`, `client`, `ui`, etc). These will be project dependent. This section mentions group-wide labels that can be used across organizations and projects.

General roles:

- `issue::soon`: Implementation on this issue has started
- `issue::review`: This issue has been implemented, and a merge request is currently being reviewed
- `merge::assignee`: Turn-based review: the assignee should check the comments and act on them
- `merge::reviewer`: Turn-based review: the reviewer·s should check the code and approve or further comment on it

Priority (recommended color: #cc338b):

- `priority::urgent`: Urgent, this needs to be done as soon as possible (stops the project from working at all, security issues…)
- `priority::high`: High, this must be done soon
- `priority::medium`: Normal, this will be done when nothing more important is waiting
- `priority::low`: Low, this will be done at some point in the future

Problem:

- `bug`: Something isn't behaving as expected
- `incident`: The service is unavailable (server down…)

Severity of bugs/incidents:

- `severity::critical`: The project is unusable until this is fixed
- `severity::major`: Some required features are broken
- `severity::moderate`: Some important features are broken
- `severity::minor`: Some features are inconvenient to use
- `severity::cosmetic`: Something is not explained correctly / doesn't display correctly, but it has no impact on usage

### Insights

Add this file to your repository for GitLab to generate the different graphs based on the labels listed above.

```yml
# .gitlab/insights.yml

issues:
  title: "Issues and bugs"
  charts:
    - title: "Priority of new issues (monthly)"
      description: "Issues created per month"
      type: stacked-bar
      query:
        issuable_type: issue
        issuable_state: opened
        group_by: month
        period_limit: 24
        collection_labels:
          - priority::urgent
          - priority::high
          - priority::medium
          - priority::low
    - title: "Priority of new bugs (monthly)"
      description: "Bugs created per month, by priority"
      type: stacked-bar
      query:
        issuable_type: issue
        issuable_state: opened
        group_by: month
        period_limit: 24
        filter_labels:
          - bug
        collection_labels:
          - priority::urgent
          - priority::high
          - priority::medium
          - priority::low
    - title: "Monthly new bugs"
      description: "Bugs created per month, by severity"
      type: stacked-bar
      query:
        issuable_type: issue
        issuable_state: opened
        group_by: month
        period_limit: 24
        filter_labels:
          - bug
        collection_labels:
          - severity::critical
          - severity::major
          - severity::moderate
          - severity::minor
          - severity::cosmetic
```

### Milestones and iterations

Use milestones to represent 'objectives' or 'versions'. Use iterations (if available) to represent 'sprints'. If iterations are unavailable, and the project doesn't have clear versions/goals, then milestones can be used as iterations.

Always set a start and due date for milestones and iterations.

### Requirements and issues

**Requirements** represent things the client wants.
**Issues** represent work that needs to be done by the team.

For each goal of the project/feature the client wants, create a requirement. Requirements should be as independent and as clear as possible. They should not link to design documents. Requirements are encouraged to reference similar requirements. Requirements should try not to depend on each other, but it is allowed if it makes requirements clearer. Reading a requirement should give exactly the information necessary to test that it is correctly implemented. Avoid editing requirements, the only exception is to add precision. When you want to add a new feature to a requirement, create a requirement that builds atop it.

Requirements are not closed, and do not disappear when they are implemented, unlike issues. The list of requirements shows what works and what doesn't (most useful for the client), the list of issues shows the work that needs to be done soon (most useful for the team).

Issues can either be used to introduce features (in which case, they will reference a requirement in one way or another), or fix an issue.

Issues that introduce features:

- To implement a simple requirement, create an issue that references it.
- To implement a complicated requirement, create an epic that references it, and use sub-issues to split the work into smaller units. If you do not have access to epics, simply create multiple issues that block each other.
- If a user requests a new feature, create a requirement for it, and then edit their issue to reference it.

Issues that fix bugs:

- Create a normal issue, as usual. No need for a requirement.

#### Requirement skeleton

```text
Extension of REQ-XXX, …
Related to REQ-XXX, …

Needed by: <type of user, if there are multiple types>

<Description, as detailed as possible>
```

#### Issue skeleton

```text
<Description>

/milestone %<milestone>
/label ~<label>
/weight 1
/estimate 1h
```

Always put a time estimate, even if it is rough. The weight is encouraged to represent a number of hours of work needed (but you can use any other system you like).

Properly label your issues.

Issues that block each other should be properly marked as such.

## Working on the project

### Non-developers

Use the issue to discuss, share ideas, share documents, etc.

### Developers

Create a branch for the issue. It should be named with the issue ID and a few words to describe it (For example, for the issue "#14 Replace Result by Either", you could name the branch `14-replace-either`). Use dashes to differentiate between words, not underscores.

As soon as possible, create a draft merge request. This will allow you to ask questions about your progress. Label your merge request properly. Update the issue's label.

When you want reviewers to look at your MR, add the `merge::reviewer` label.

Follow the [BrainDot coding style](https://gitlab.com/braindot/legal/-/tree/master/coding-style), including for commits.

When a reviewer requests changes:

- Do not hesitate to ask for clarifications
- If you understand what is needed and why, create a new commit to fix the issue, then rebase it.
- When you're done with everything, switch the label to `merge::reviewer`. Do not resolve threads. If you want to make it clear that you saw a thread, add a "done" comment, or similar.

### Reviewers

Read the code. Check that the commits correspond to the coding style. If the commits are properly made, you should be able to review the branch commit per commit and never be confused.

When you request changes, add the label `merge::developper`.

Your comments should be clear as to what the issue is. If a problem happens many times (for example, the coding style), create a single comment for the whole file.

Do not bother with things that can be automated (pipeline failure, coding style…), these are the developer's responsibility.

## Utilities

The following links can give you a personal dashboard of what needs to be done; replace `<username>` by your username.

My todo list:
`https://gitlab.com/dashboard/todos`

Issues I should start working on:
`https://gitlab.com/dashboard/issues?scope=all&utf8=%E2%9C%93&state=opened&assignee_username=<username>&not[label_name][]=issue%3A%3Areview&not[label_name][]=issue%3A%3Adoing`

Merge requests I should be working on:
`https://gitlab.com/dashboard/merge_requests?scope=all&utf8=%E2%9C%93&state=opened&assignee_username=<username>&not[label_name][]=merge%3A%3Areviewer`

Merge requests I should be reviewing:
`https://gitlab.com/dashboard/merge_requests?scope=all&utf8=%E2%9C%93&state=opened&reviewer_username=<username>&not[label_name][]=merge%3A%3Adeveloper&draft=no&not[label_name][]=issue%3A%3Asoon&not[label_name][]=issue%3A%3Adoing&not[label_name][]=issue%3A%3Areview`
