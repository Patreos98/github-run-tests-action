![mabl logo](https://avatars3.githubusercontent.com/u/25963599?s=100&v=4)

# mabl GitHub Run Tests Deployment Action

This GitHub Action creates a mabl deployment event, triggering any functional
tests associated with that deployment and waiting for their results.

### Example workflow:

```
on: [push]

name: mabl

jobs:
  test:
    name: mabl Test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master

      - name: Functional test deployment
        id: mabl-test-deployment
        uses: mablhq/github-run-tests-action@v1.4
        env:
          MABL_API_KEY: ${{ secrets.MABL_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          application-id: <your-application-id-a>
          environment-id: <your-environment-id-e>
```

### Environment variables

- `MABL_API_KEY` {string} - Your mabl API key
  [available here](https://app.mabl.com/workspaces/-/settings/apis) This should
  be installed as a secret in your github repository.
- `GITHUB_TOKEN` {string} (optional) - The Github token for your repository. If
  provided, the mabl action will associate a pull request with the deployment if
  the commit being built is associated with any pull requests. This token is
  automatically available as a secret in your repo but must be passed in
  explicitly in order for the action to be able to access it.

### Inputs

**Note**: Either `application-id` or `environment-id` must be supplied.

- `application-id` {string} (optional) - mabl id for the deployed application.
  Use the
  [curl builder](https://app.mabl.com/workspaces/-/settings/apis#api-docs-selector-dropdown-button)
  to find the id.
- `environment-id` {string} (optional) - mabl id for the deployed environment.
  Use the
  [curl builder](https://app.mabl.com/workspaces/-/settings/apis#api-docs-selector-dropdown-button)
  to find the id.
- `browser-types` {string} {optional}: comma seperated override for browser
  types to test e.g. `chrome, firefox, safari, internet_explorer`. If not
  provided, mabl will test the browsers configured on the triggered test.
- 'uri' {string} {optional} the base uri to test against. If provided, this will
  override the default uri associated with the environment in mabl
- `rebaseline-images` {boolean} (optional) - Set `true` to reset the visual
  baseline to the current deployment
- `set-static-baseline` {boolean} {optional} - Set `true` to use current
  deployment as an exact static baseline. If set, mabl will **not** model
  dynamic areas and will use the current deployment as the pixel-exact visual
  baseline.
- `continue-on-failure` {boolean} {optional} - Set to true to continue the build
  even if there are test failures
- `event-time` {int64} (optional) - Event time the deployment occurred in UTC
  epoch milliseconds. Defaults to now.

### outputs:

- `mabl-deployment-id` {string} - mabl id of the deployment
- `plans_run` {int32} - number of mabl plans run against this deployment. A mabl
  plan is a collection of similarly configured tests.
- `plans_passed` {int32} - number of mabl plans that passed against this
  deployment. A mabl plan is a collection of similarly configured tests.
- `plans_failed` {int32} - number of mabl plans that failed against this
  deployment. A mabl plan is a collection of similarly configured tests.
- `tests_run` {int32} - total number of mabl tests run against this deployment.
- `tests_passed` {int32} - number of mabl tests that passed against this
  deployment.
- `tests_failed` {int32} - number of mabl tests that failed against this
  deployment.
