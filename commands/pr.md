Use `gh` command to create a new PR to the upstream branch $ARGUMENT.

1. Use the .github/pull_request_template.md as PR template
2. Create the PR to the upstream $ARGUMENT branch
3. If the current branch is named with pattern "AFD-*", add "[AFD-xxxx]" to the beginning of the PR title.
4. If $ARGUMENT is empty, use `master` as default.
5. When filling the section of "目的或是相关联的 Jira tickets", generate a link of Jira ticket beginning with "https://aftership.atlassian.net/browse/AFD-xxxx"
