### IAM (Identity and Access Management)

1. IAM Users
  - Represent an individual person(developer, tester)
  - Can have username + password(console access)
  - Can have access keys(CLI access)
  - With specific permissions

2. IAM groups
  - A collection of users with shared permissions.
  - Example:
      - Group: Developers -> Policy: PowerUserAccess
      - Group: Viewers -> Policy: ReadOnlyAccess

3. IAM Roles
  - Temporary identities assumed by users, apps or AWS services.
  - No permanent credentials (keys are short-lived)
  - Best practice for EC2, Lambda, EKS, CI/CD

4. IAM Policies:
  - JSON documents defining permissions.
