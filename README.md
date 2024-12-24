# Description

prepare-commit-msg git hook that parses jira ticket name from branch name and prepends it to commit message.
in case of empty message it preprends jira ticket name and allows to edit message afterwards.
if git gets a commit message without jira ticket name in the first line then it wil prepend
