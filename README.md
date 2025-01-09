# Stackgen CLI GitHub Action

This action allows you to run Stackgen CLI commands in your GitHub workflows.

## Inputs

### Required Inputs

- `command`: The main stackgen command to run (appstack, provision, destroy, etc)

### Optional Inputs

- `subcommand`: Subcommand for the main command (if applicable)
- `flags`: Command flags as a multi-line string, one flag per line
- `aws_access_key`: AWS Access Key for authentication
- `aws_secret_key`: AWS Secret Key for authentication
- `aws_region`: AWS Region

## Example Usage

```yaml
- uses: your-org/stackgen-action@v1
  with:
    command: 'provision'
    flags: |
      --appstack-id my-stack-id
      --apply
      --work-dir ./infrastructure
      --log 3
      --output human
      --var environment=production
      --var region=us-west-2
      --backend-config bucket=my-terraform-state
    aws_access_key: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws_secret_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws_region: 'us-west-2'
```

## Advanced Usage - Multiple Commands

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Generate Infrastructure
        uses: your-org/stackgen-action@v1
        with:
          command: 'generate'
          flags: |
            --work-dir ./infrastructure
          
      - name: Provision Infrastructure
        uses: your-org/stackgen-action@v1
        with:
          command: 'provision'
          flags: |
            --apply
            --work-dir ./infrastructure
          aws_access_key: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws_secret_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

Each flag in the `flags` input should be on a new line, including its value if applicable. This provides a more straightforward and readable way to pass command-line arguments to the Stackgen CLI.