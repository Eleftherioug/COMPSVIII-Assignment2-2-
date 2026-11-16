# GitHub Actions Workflow Analysis

1. **What triggers this workflow to run?**  
   The workflow runs when code is pushed to `main` and when a pull request targets `main`.

2. **What are the four main steps this workflow performs?**  
   The `build-and-test` job checks out the repository, validates HTML, checks links, and uploads the site as a Pages artifact. The link-check step has `continue-on-error: true`, so a link-check failure is reported but does not stop the job. The separate `deploy` job then deploys the artifact to GitHub Pages when the build job succeeds and the event is a push to `main`.

3. **What does the "Checkout code" step do and why is it necessary?**  
   `actions/checkout@v4` checks out the repository contents into the runner's workspace. The later validation, link-check, and artifact steps need those files to run.

4. **What is the purpose of the environment configuration?**  
   The deploy job targets the `github-pages` environment and records the published page URL from the deployment step. The job also requests the Pages and OIDC permissions required to publish the site.

5. **How does this automated deployment improve reliability compared to manual deployment?**  
   Every qualifying update is checked and packaged through the same repeatable workflow. Deployment waits for the build-and-test job to succeed, reducing inconsistent manual steps and catching HTML issues before publishing.

6. **What would happen if you pushed code to a different branch (not `main`)?**  
   A push to another branch would not trigger this workflow because the push trigger only includes `main`. A pull request from that branch targeting `main` would trigger the build-and-test job, but the deploy job would not run for a pull request.
