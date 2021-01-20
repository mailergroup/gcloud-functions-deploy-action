# gcloud-functions-deploy-action

Builds and deploys a HTTP-triggered Google Cloud Function.

# Required Inputs: *

**service_account_key** - A Google service account key that has at least `roles/cloudfunctions.admin` and `roles/iam.serviceAccountUser`.

**project** - The Google account project

**region** - The GCP region where Cloud function will deploy

**runtime** - Runtime that will be used for the Cloud function. Available options can be found (here)[https://cloud.google.com/sdk/gcloud/reference/functions/deploy#--runtime]

**entry_point** - The entrypoint function, without () (i.e. `run`, not `run()`)

**service_account** - The service account email used by the function

# Optional Inputs:

Based on what you wish to do in this action.

**env_vars** - Environment variables for the function, in `KEY=VALUE,KEY1=VALUE1` format

**vpc_connector** - Which VPC connector to use

**max_instances** - Max concurrent instances of the function allowed to exist

**memory** - Limit of the amount of memory the function can use

**timeout** - Timeout for the Cloud function (in seconds) - defaults to 30

**egress_settings** - Egress settings. Available options:
  - private-ranges-only
  - all

**ingress_settings** - Ingress settings - defaults to `internal-and-gclb`. Available options:
  - internal-and-gclb
  - internal-only
  - all

# Example usage

``` yaml
- uses: remotecompany/gcloud-functions-deploy-action@main
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "remotecompany"
    name: "my-cloud-function"
    region: "europe-west1"
    runtime: "python38"
    entry_point: "run"
    service_account: "github-actions@remotecompany.iam.gserviceaccount.com"
    env_vars: SECRET1=${{ secrets.SECRET1 }},SECRET2=${{ secrets.SECRET2 }}
    vpc_connector: "my-vpc-connector"
    max_instances: 1
    memory: "128MB"
    timeout: 30
    egress_settings: private-ranges-only
    ingress_settings: internal-and-gclb
```
