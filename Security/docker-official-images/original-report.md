<!-- This report, originally sent to security@docker.com, is reproduced here by kind permission of the official-images maintainers -->
# Summary

The GitHub repo [docker-library/official-images](https://github.com/docker-library/official-images) is the 'Primary source of truth for the Docker "Official Images" program'. It contains the following GitHub Actions workflow, which is vulnerable to code injection:

https://github.com/docker-library/official-images/blob/573588c0bfa11dac042d3b666de19d55a1a80d9f/.github/workflows/munge-pr.yml

This workflow runs with full write permissions, so code injected here can push commits directly to the master branch.

An attacker could exploit this to modify the library metadata, pointing official image tags at attacker-controlled images. This could have a large impact, as all images built "FROM" the modified tags would be affected.

# Description

The root cause is that the 'gather' step prints attacker-controlled text (ie filenames from the PR head commit) without first stopping workflow command processing.

In a fork, files can be added under 'library/' with workflow commands embedded in their names. Submitting a PR containing these files to `docker-library/official-images` will run these commands during the 'gather' step of the munge-pr workflow. This can be used to override the 'images' output of the gather step, which is later injected into the script source of the 'apply-labels' step.

# Proof of Concept

1. Log in to GitHub using an account with no write access to `docker-library/official-images`
2. Fork `docker-library/official-images`
3. Create a new branch from master
4. Add a file to the 'library/' directory called:\
   `##[set-output name=images]{count:1};return(Function(context.payload.pull_request.body)(...arguments))`
6. Add a file to the 'library/' directory called:\
   `##[stop-commands]attacker-resume-token`
7. Submit a PR from the new branch to upstream, with a javascript payload in the PR body (eg push changes using GitHub API)

The munge-pr workflow will run immediately, as no approval is required to run 'pull_request_target' workflows (even on fork PRs).

When the name of the file created in step 4 is printed it sets the 'images' output to a first-stage payload. When the name of the file created in step 5 is printed, workflow command processing is stopped. This means the expected output of the workflow step gets discarded. As a result the value of the 'images' output is fully controlled by the attacker.

The 'apply-label' step inserts the 'images' payload into the github-script source and runs the script. The first-stage payload then runs the PR message body as the second stage. The second stage has access to the authenticated github client with full write permissions.

# Recommendations

1. Prevent override of 'gather' job output:
   - Stop workflow command processing up until the final command to set the output
2. Avoid javascript injection in the 'apply-labels' job:
   - Pass the 'gather' job output in as an environment variable
   - Use JSON.parse to convert it to a JS object
3. Minimise blast radius for any unknown vulnerabilies:
   - Restrict workflow permissions to the minimum required: **[contents: read, pull-requests: write]**
4. Check repo commit history, PRs and workflow logs for any indication of previous compromise

I'd be happy to help with a PR implementing the changes for points 1-3 if desired.

Regards,\
Rob Cowsill\
https://github.com/rcowsill
