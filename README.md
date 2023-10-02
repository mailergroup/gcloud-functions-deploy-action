# gcloud-functions-deploy-action

Builds and deploys a HTTP- or Pub/Sub-triggered Google Cloud Function.

# Required Inputs: *

**service_account_key** - A **base64-encoded** Google service account key that has at least `roles/cloudfunctions.admin` and `roles/iam.serviceAccountUser`.

**project** - The Google account project

**region** - The GCP region where Cloud function will deploy

**runtime** - Runtime that will be used for the Cloud function. Available options can be found (here)[https://cloud.google.com/sdk/gcloud/reference/functions/deploy#--runtime]

**entry_point** - The entrypoint function, without () (i.e. `run`, not `run()`)

**service_account** - The service account email used by the function

(DEPRECATED) ~**trigger_type** - The kind of GCF trigger (`http` or `pubsub`)~

# Optional Inputs:

Based on what you wish to do in this action.

**env_vars** - Environment variables for the function, in `KEY=VALUE,KEY1=VALUE1` format

**vpc_connector** - Which VPC connector to use

**max_instances** - Max concurrent instances of the function allowed to exist

**memory** - Limit of the amount of memory the function can use

**timeout** - Timeout for the Cloud function (in seconds) - defaults to 30

**trigger_topic** - The Pub/Sub topic to subscribe to. If this is enabled, Pub/Sub trigger is assumed. 

**egress_settings** - Egress settings for VPC connector. Will only be applied if `vpc_connector` is also specified. Available options:
  - private-ranges-only
  - all

**ingress_settings** - Ingress settings - defaults to `internal-and-gclb`. Available options:
  - internal-and-gclb
  - internal-only
  - all

# Example usage

## HTTP-triggered GCF with VPC connector

``` yaml
- uses: mailergroup/gcloud-functions-deploy-action@v1
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "mailergroup"
    name: "my-cloud-function"
    region: "ZONE"
    runtime: "python38"
    entry_point: "run"
    service_account: "SERVICE_ACCOUNT_NAME"
    env_vars: SECRET1=${{ secrets.SECRET1 }},SECRET2=${{ secrets.SECRET2 }}
    vpc_connector: "my-vpc-connector"
    max_instances: 1
    memory: "128MB"
    timeout: 30
    egress_settings: private-ranges-only
    ingress_settings: internal-and-gclb
```

## Pub/Sub topic triggered GCF

```yaml
- uses: mailergroup/gcloud-functions-deploy-action@v1
  with:
    service_account_key: ${{ secrets.GOOGLE_SERVICE_KEY }}
    project: "mailergroup"
    name: "my-cloud-function"
    region: "ZONE"
    runtime: "python38"
    entry_point: "run"
    service_account: "SERVICE_ACCOUNT_NAME"
    env_vars: SECRET1=${{ secrets.SECRET1 }},SECRET2=${{ secrets.SECRET2 }}
    max_instances: 1
    memory: "128MB"
    trigger_topic: my-pubsub-topic
    timeout: 30
    ingress_settings: internal-and-gclb
```
