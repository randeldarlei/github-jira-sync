# Jira Sync Workflow
This project performs real-time synchronizations od issues from Github to Jira.

### How Workflow Works?
When an Issue is opened or updated, the Workflow triggers the creation of a Task on your Jira board, adds or updates comments, and also deletes them. The Workflow just needs to be present in the '.github' folder. It's mandatory for the folder to be in the root directory, otherwise the process will fail.

- You can read more about Atlassian API here
  https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#version

### Sync Issues
- In the Jira console, access your project's board and in the left side menu, click "Permission Settings".
- Then click on "People" and "Add People".
- Add the "your-team-user" account and grant Administrator and Team Member permissions.
- Add your Jira project key in the field below:

name: Create Issue in Jira 
        if: ${{ github.event_name == 'issues' && github.event.action == 'opened' }}
        id: create_jira_issue
        run: |
          summary=$(cat summary.txt)
          description=$(cat description.txt)
          description=$(echo "${description}" | tr -d '\n' | tr -d '\r')
          token=$(cat auth_token.txt)

          response=$(curl -v --request POST \
                    --url 'https://<your-project-key>/rest/api/3/issue' \
                    --header "Authorization: Basic $token" \
                    --header 'Accept: application/json' \
                    --header 'Content-Type: application/json' \
                    --data '{
                      "fields": {
                        "project": {
                          "key": "project-key-here"