# Adding a new Reusable Workflow

As the number of terraform modules grew it was clear to our developers that maintaining the modules without automation was a mammoth task.

To simplify and speed up terraform module releases we have put together automation which removes steps from the module coders and speeds up the all process of releasing changes to modules.

This document breaks down the process of adding new reusable workflows to be used in our terraform modules.

To add a new reusable workflow, follow these steps.

1. Clone terraform-module-support repository in GitHub.
```console
$ git clone git@github.com:boldlink/terraform-module-support.git
```
2. Checkout to a new branch, e.g `feature/auto-merge-workflow`.
3. Define the workflow steps and jobs to be executed. Be sure to define the workflow will run on workflow_call. This is because the workflow will only be run when referenced by a caller workflow, not in this repository.
4. Ensure the workflow has a slack notify step in case the workflow fails to run successfully. Sample code is as below.
```console
    - name: Slack Notification failure
      if: failure()
      uses: rtCamp/action-slack-notify@v2
      env:
        SLACK_USERNAME: Boldlinksig
        SLACK_MESSAGE: ":bell: Job failed in ${{ github.event.repository.name }} repository."
        SLACK_FOOTER: "Boldlink-SIG ${{ steps.year.outputs.year }}"
        SLACK_COLOR: ${{ job.status }} 
        SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
```

5.Create a PR for terraform-module-support and post the link to slack devops-teams channel for someone to review it and approve for you.

**NOTE:** Ensure you test the new workflow before creating and submitting a PR since you risk releasing a bug to all terraform modules.
