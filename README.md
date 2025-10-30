# amplify-build-status

amplify-build-status is a GitHub action to check the build status of an app on AWS Amplify.

It can also be used to wait until a build is complete before continuing the workflow.
If the build fails the workflow will fail (unless set otherwise).

Example:

```
- name: Wait for Amplify to finish remote build
  uses: duckbytes/amplify-build-status@v2.2
  id: amplify_status
  with:
    app-id: ${{ secrets.AMPLIFY_APP_ID }}
    branch-name: ${{ github.ref_name }}
    commit-id: ${{ github.sha }}
    wait: true
```

This step when run on a branch connected to the Amplify console will stop the workflow from continuing until the remote build is complete.

You can access the app ID by going to Amplify on the AWS console:

Open your app on the list, and go to the _Backend environments_ tab.

Click on _Edit backend_ and copy the appId value.

### Inputs

`app-id`, `branch-name` and `commit-id` are all required input. You must run aws-actions/configure-aws-credentials@v3 to get credentials before using this action.

Other inputs are:

- `wait` # Whether or not to wait until the remote build is completed or failed before continuing the workflow (default: false).
- `timeout` # How long to wait in minutes for the remote build to complete or fail, if `wait` is set (default: 120. 0 is no timeout).
- `no-fail` # If the remote build fails the script will still use a successful exit status and not interrupt the workflow (default: false).

### Outputs

- `status` # The build status output according to the AWS CLI.
- `backend_environment` # The environment name.
- `graphql_endpoint` # The GraphQL endpoint.

You can access these values later in your workflow like this:

`${{ steps.amplify_status.outputs.environment_name }}`

## Known issues

Sometimes the Amplify console doesn't automatically associate a backend with a new deployment.

Amplify will successfully complete the new build, but it is necessary click `(Edit)` next to "Continuous deploys set up" and set it to the correct backend before the action can return the environment name and GraphQL endpoint.
