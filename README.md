# Google Cloud SQL Proxy

Github action which will start a [Google Cloud SQL Proxy](https://cloud.google.com/sql/docs/postgres/sql-proxy). 

## Prerequisites

Set up the following resources manually in the Cloud Console 
or use a tool like [Terraform](https://www.terraform.io).

- Running Cloud SQL instance with a public IP address
- Create Service Account with Role `Cloud SQL Client` and do authentication via:
  - [Workload Identity Federation](https://github.com/google-github-actions/auth#with-workload-identity-federation) 
  - ~~[Long-lived Service Account Key JSON credential](https://github.com/google-github-actions/auth#authenticating-via-service-account-key-json)~~  ***Deprecated***


## Github Action Inputs

| Variable                         | Description                                                                 |
|----------------------------------|-----------------------------------------------------------------------------|
| `creds`                          | Automatically exported as a short-lived Service Account JSON Key            |
| `instance`                       | ***Required*** Cloud SQL connection name                                    |
| `port`                           | Listen on port, default 5432                                                |
| `version`                        | Cloud SQL Proxy version, default latest                                     |


## Example Usage

```
jobs:
  job_id:
    # ...

    # Add "id-token" with the intended permissions.
    permissions:
      contents: 'read'
      id-token: 'write'

    steps:
    - uses: 'actions/checkout@v3'

    - id: 'auth'
      name: 'Authenticate to Google Cloud'
      uses: 'google-github-actions/auth@v0'
      with:
        workload_identity_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
        service_account: 'my-service-account@my-project.iam.gserviceaccount.com'

    - uses: wagnerpereira/gce-cloudsql-proxy-action@v2
      with:
        instance: my-project:us-central1:instance-1
```

