# WikiVote

[WikiVote](https://wikidev.org) is a GitHub application that allows holding a vote on issues and pull requests.

## Introduction

This chart bootstraps a WikiVote deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

Before installing this chart, you need to [setup a GitHub application](https://docs.github.com/en/apps/creating-github-apps/creating-github-apps/creating-a-github-app). The application requires the following permissions [to be set](https://docs.github.com/en/apps/maintaining-github-apps/editing-a-github-apps-permissions):

Repository:

- **Checks**: *read/write*
- **Contents**: *read*
- **Issues**: *read/write*
- **Metadata**: *read*
- **Pull requests**: *read/write*

Organization:

- **Members**: *read*

In addition to those permissions, it must also be subscribed to the following events:

- *Issue Comment*
- *Issues*
- *Pull Request*

WikiVote expects GitHub events to be sent to the `/api/events` endpoint. In the GitHub application, please enable `webhook` and set the target URL to your exposed endpoint (ie: <https://example.com/api/events>). You will need to define a random secret for the webhook (you can use the following command to do it: `openssl rand -hex 32`). Please note your webhook secret, as well as the GitHub application ID and private key, as you'll need them in the next step when installing the chart.

Once your GitHub application is ready and WikiVote has been deployed, you can install it in the organizations or repositories you need.

## Installing the chart

Create a values file (`my-values.yaml`) that includes the configuration values required from your GitHub application:

```yaml
wikivote:
  github:
    appID: 123456 # Replace with your GitHub app ID
    appPrivateKey: |-
      -----BEGIN RSA PRIVATE KEY-----
      ...
      YOUR_APP_PRIVATE_KEY
      ...
      -----END RSA PRIVATE KEY-----
    webhookSecret: "your-webhook-secret"
```

To install the chart with the release name `my-wikivote` run:

```bash
$ helm repo add wikivote https://wikidevllc.github.io/wikivote/
$ helm install --values my-values.yaml my-wikivote wikivote/wikivote
```

The command above deploys WikiVote on the Kubernetes cluster using the default configuration values and the GitHub application configuration provided. Please see the [chart's default values file](https://github.com/cncf/wikivote/blob/main/charts/wikivote/values.yaml) for a list of all the configurable parameters of the chart and their default values.

## Uninstalling the chart

To uninstall the `my-wikivote` deployment run:

```bash
$ helm uninstall my-wikivote
```

This command removes all the Kubernetes components associated with the chart and deletes the release.

## How WikiVote works

For more information about how WikiVote works from a user's perspective please see the [repository's README file](https://github.com/cncf/wikivote#readme).
