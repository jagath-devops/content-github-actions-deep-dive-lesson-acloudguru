Additional Resources
Scenario
Your team has set up an automated build process for your application code. You want to store this artifact so other teams can access it for downstream work. You will need to build out the workflow file to leverage GitHub Packages to store new versions of your code with each new build.

Objectives
Set up a GitHub repository.
Create a release.
Add a release asset.
Links and Resources:
GitHub repository: https://github.com/linuxacademy/content-github-actions-deep-dive-lesson

GitHub Actions Checkout community action: https://github.com/marketplace/actions/checkout

Upload/download artifacts: https://docs.github.com/en/actions/guides/storing-workflow-data-as-artifacts

Using needs: https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idneeds

Step ids: https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idoutputs

Git tags: https://git-scm.com/book/en/v2/Git-Basics-Tagging

GitHub releases: https://docs.github.com/en/github/administering-a-repository/releasing-projects-on-github/managing-releases-in-a-repository

Learning Objectives
0 of 3 completed


Set up a GitHub Repository

Fork the repo https://github.com/linuxacademy/content-github-actions-deep-dive-lesson.
Check out the lab branch and confirm /github/workflows/deploy-pipeline.yaml contains lint and build jobs.
Note: You can clone the repo locally or work in the GitHub browser.

If you have trouble, the complete code for one possible solution is on the lab-package-solution.


Create a Release

Create a publish job.
Add a step to create a release and commit the changes.
Check the release in the GitHub console. It has the complete codebase, but not the build artifact with the dependencies.

Add a Release Asset

Set the publish job to run after the build job.
Add a step to the publish job to upload the build artifact as a part of the release and commit the changes.
Note: The new release will contain a zipped bundle with all the libraries.



















Creating a Release with GitHub Actions
Introduction
Your team has set up an automated build process for your application code. You want to store this artifact so other teams can access it for downstream work. You will need to build out the workflow file to leverage GitHub Packages to store new versions of your code with each new build.

Solution
Open the lab's provided GitHub repository.

If you don't already have a personal GitHub account, create one before you start the lab by signing up at github.com.

Set up a GitHub Repository
At the top right of the main page, click Fork .
Select your personal GitHub profile to fork the repo into your own account and ensure to deselect the "Copy the main branch only" option.
On the top menu, click Actions.
Click View the workflows directory.
Select the init.yaml placeholder file and view its contents.
Click Actions to return to the previous page.
Select I understand my workflows, go ahead and enable them.
Click the Code tab to review the code.
Click the branch dropdown and select lab.
Review the contents of the lab branch and confirm /github/workflows/deploy-pipeline.yaml contains lint and build jobs.
Note: You can clone the repo locally or work in the GitHub browser.

If you have trouble, the complete code for one possible solution is on the lab-package-solution.

Create a Release
From inside deploy-pipeline.yaml, click the pencil icon on the right to edit the file in the browser.

Scroll down to the bottom and add a few spaces.

Create a new publish job:

Note: Ensure the indentation is correct (none of the job's text should be underlined in red). Otherwise, you'll get an error when you try to run the job.

publish:
  runs-on: ubuntu-latest
  steps:
    - name: Create release
      id: create_release
      uses: actions/create-release@v1
      env:
        GITHUB_TOKEN: ${{ secrets.github_token }} # This token is provided by Actions, you do not need to create your own token
      with:
        tag_name: ${{ github.run_number }}
        release_name: Release ${{ github.run_number }}
        body: New release for ${{ github.sha }}. Release notes can be found in the docs.
        draft: false
        prerelease: false
At the top right, click Start commit and select Commit changes.

Select View runs to view the executed deployment pipeline.

Note: Deployment may take several minutes.

Click the Code tab to return to the main code page.

On the right under Releases, click Release (latest) to view our release assets.

Add a Release Asset
Return to the deploy-pipeline.yaml file and click the pencil icon. to

At the bottom of our publish workflow, add a new step to download an artifact:

- name: Download artifact
    uses: actions/download-artifact@v2
    with:
      name: zipped-bundle
Add the required build dependency to our publish workflow:

publish:
  runs-on: ubuntu-latest
  needs: build
Add a new step to upload a build artifact as part of the release:

- name: Upload release asset
  uses: actions/upload-release-asset@v1
  env:
    GITHUB_TOKEN: ${{ secrets.github_token }}
  with:
    upload_url: ${{ steps.create_release.outputs.upload_url }}
    asset_path: ./${{ github.sha }}.zip
    asset_name: source_code_with_libraries.zip
    asset_content_type: application/zip
Click Start commit > Commit changes.

Click View runs.

Select the latest Update deploy-pipeline.yaml event to view the current run.

Once the workflow has finished executing, select publish to view the output.

At the top left, click the Code tab to return to the main code page.

Under Releases, select Release for 2 to view the release assets.

Note: The new release will contain a zipped bundle with all the libraries.

Conclusion
Congratulations — you've completed this hands-on lab!# GitHub Actions Deep Dive

A simple Lambda function that can be deployed via a GitHub Actions Workflow. 
