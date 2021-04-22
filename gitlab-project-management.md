# Managing projects with GitLab

This document takes inspiration from:

- [The guidelines for GitLab itself](https://docs.gitlab.com/ee/development/code_review.html)

## Labels

For issues:

- Create labels `issue::soon` (backlog), `issue::doing` (currently working on it) and `issue::review` (a non-draft merge request exists).
- Create labels that represent the different parts of the project (for example, `client`, `server`, `ui`…)
- Create labels `merge::developer` and `merge::reviewer` for turn-based review.

## Milestones and iterations

Use milestones to represent 'objectives' or 'versions'. Use iterations (if available) to represent 'sprints'. If iterations are unavailable, and the project doesn't have clear versions/goals, then milestones can be used as iterations.

Always set a start and due date for milestones and iterations.

## Requirements and issues

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

### Requirement skeleton

```text
Extension of REQ-XXX, …
Related to REQ-XXX, …

Needed by: <type of user, if there are multiple types>

<Description, as detailed as possible>
```

### Issue skeleton

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
