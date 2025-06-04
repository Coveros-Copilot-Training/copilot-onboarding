# Organization Cleanup Workflow

This repository includes an automated workflow that removes non-admin users from the GitHub organization every night at midnight.

## Workflow: `cleanup-organization.yml`

### Purpose
- Removes all organization members who are not owners/admins
- Runs automatically every night at midnight UTC
- Sends email notifications to organization owners/admins when users are removed

### Features
- **Scheduled Execution**: Runs daily at midnight UTC via cron schedule
- **Manual Trigger**: Can be triggered manually via `workflow_dispatch` for testing
- **Safe Operation**: Only removes non-admin users, preserves organization owners/admins
- **Email Notifications**: Prepares email digest with removed user details (only if users were removed)
- **Error Handling**: Logs failed removals and continues processing other users
- **Audit Trail**: Detailed logging of all operations for compliance and debugging

### Prerequisites

1. **Personal Access Token (PAT)**: The workflow requires a GitHub Personal Access Token with organization management permissions stored as `secrets.PAT`

2. **Repository Permissions**: The workflow needs `write-all` permissions to manage organization membership

3. **Email Service Integration** (Optional): To actually send emails, integrate with one of these services:
   - SendGrid using `dawidd6/action-send-mail` action
   - AWS SES via `aws-actions/configure-aws-credentials`
   - Custom email service API
   - GitHub repository dispatch to trigger separate email workflow

### Setup Instructions

1. **Create Personal Access Token**:
   - Go to GitHub Settings > Developer settings > Personal access tokens
   - Create a token with `admin:org` scope
   - Add it as a repository secret named `PAT`

2. **Configure Email Service** (if desired):
   - Choose an email service provider
   - Add necessary credentials as repository secrets
   - Modify the "Send email notification" step to use your chosen service

### Security Considerations

- The workflow only removes **non-admin** users to prevent accidental removal of organization owners
- All operations are logged for audit purposes
- Failed removals are tracked and reported
- The workflow requires explicit permissions and authentication

### Testing

Use the manual trigger (`workflow_dispatch`) to test the workflow before relying on the scheduled execution.

### Monitoring

Check the workflow run logs to:
- Verify correct identification of admin vs non-admin users
- Monitor removal success/failure rates
- Review email notification content
- Ensure no unauthorized removals

## Workflow Integration

This cleanup workflow complements the existing `onboard.yml` workflow which handles adding users to the organization.