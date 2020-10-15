# Anthos Developer Sandbox

## Introduction

This tutorial allows you to get your feet wet with Anthos development practices
like continuous/iterative development, local builds and local deployments.

By the end of this tutorial you will have:

1.  Run this app on a local Kubernetes cluster with minikube
2.  Use the Cloud Build local builder to run tests locally
3.  Use Cloud Code to quickly test, debug and iterate on changes
4.  Build the application with Cloud Buildpacks
5.  Deploy the app to the Cloud Run Local Emulator

## Running your app in a local cluster

Our sample application is already available to you. Let's run it in a local
Kubernetes cluster in Cloud Shell:

*   In your
    <walkthrough-editor-spotlight spotlightId="menu-terminal-new-terminal">terminal</walkthrough-editor-spotlight>,
    run the command below:

    ```bash
    minikube start -p cloud-code-minikube
    ```

*   If prompted, authorize Cloud Shell to make Google Cloud API calls

*   Once your local cluster in Cloud Shell is set-up, you will see the following
    message

    ```terminal
    Done! kubectl is now configured to use "minikube" by default
    ```

Let's now build and run this app:

*   Click
    <walkthrough-editor-spotlight spotlightId="cloud-code-status-bar">Cloud
    Code</walkthrough-editor-spotlight> in the status bar
*   Select <walkthrough-editor-spotlight spotlightId="cloud-code-run-on-k8s">Run
    on Kubernetes</walkthrough-editor-spotlight>, confirm that you want to use
    the "minikube" context
*   An
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel will pop-up after a few seconds, displaying the progress as your app
    is built/deployed
*   Once your app is built (will take a few minutes), launch it with the Web
    Preview link displayed in your
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel, near the text:

    ```terminal
    Forwarded URL from service anthos-sandbox-sample-application-external:
    ```

Congratulations! You've just run your first app on Kubernetes using
[Cloud Code](https://cloud.google.com/code). Next step, let's modify and
recompile this app.

## Use the Cloud Build local builder to run tests locally

Let's now run the tests for our application locally. We'll do this with the
Cloud Build local builder.

*   In your
    <walkthrough-editor-spotlight spotlightId="menu-terminal-new-terminal">terminal</walkthrough-editor-spotlight>,
    run the following command to run the tests
    (<walkthrough-editor-open-file filePath="tests/test_application.py">test_application.py</walkthrough-editor-open-file>).

    ```bash
    cloud-build-local --dryrun=false .
    ```

*   You'll see that the tests have failed:

    ```terminal
    ## FAIL: test_my_app (tests.test_application.TestMyApplication)

    Traceback (most recent call last): File
    "/workspace/tests/test_application.py", line 26, in test_my_app assert
    response.data == b"Hello Anthos" AssertionError

    ----------------------------------------------------------------------------

    Ran 1 test in 0.021s

    FAILED (failures=1)
    ```

Looks like we were expecting our application to say "Hello Anthos" instead of
"Hello World". Let's fix that in the next step.

## Editing and Recompiling your app

Our application is composed of the following:

*   A basic web app
    (<walkthrough-editor-open-file filePath="src/main.py">main.py</walkthrough-editor-open-file>),
    which returns "Hello, world!" to all received requests
*   A load balancer, "anthos-sandbox-sample-application-external"
    [Kubernetes Service](https://kubernetes.io/docs/concepts/services-networking/service/)
    (<walkthrough-editor-open-file filePath="kubernetes-manifests/hello.service.yaml">hello.service.yaml</walkthrough-editor-open-file>),
    which exposes our app to the internet.

Cloud Code enables iterative development so your deployed app updates as you
write code. To modify and automatically recompile our app:

*   Change
    <walkthrough-editor-select-line filePath="src/main.py" startLine="9" endLine="9" startCharacterOffset="19" endCharacterOffset="24">main.py</walkthrough-editor-select-line>
    to print "Hello, Anthos!". The file will save automatically
*   You'll notice in your
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel that your app automatically starts rebuilding
*   Once your app finishes building and deploying, view the updated app text by
    clicking the link in your
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel or by returning to your open tab and refreshing it

## Build the application with Cloud Buildpacks

Now that we've fixed the bug, let's re-run the tests:

*   In your
    <walkthrough-editor-spotlight spotlightId="menu-terminal-new-terminal">terminal</walkthrough-editor-spotlight>,
    run the following command from before:

    ```bash
    cloud-build-local --dryrun=false .
    ```

*   You'll see that the tests are now passing:

    ```terminal
    Ran 1 test in 0.020s

    OK
    ```

*   We've also successfully built a container image with
    [Google Cloud Buildpacks](https://github.com/GoogleCloudPlatform/buildpacks).
    You can verify that your image is built by entering the following command
    and viewing the details under the `LOCAL` output:

    ```bash
    pack inspect-image anthos-sandbox-sample-application
    ```

Next, we'll deploy the app to the Cloud Run Emulator.

## Deploy the app to the Cloud Run Emulator

Now that we've built an image with Cloud Buildpacks, we can choose to deploy it
directly to Cloud Run to take advantage of it's ability to scale up whenever
traffic increases, and scale to zero when it's not being used, eliminating any
idle costs.

We can get the same experience as deploying to Cloud Run by redeploying our app
locally on the Cloud Run Emulator:

*   Launch
    <walkthrough-editor-spotlight spotlightId="cloud-code-status-bar">Cloud
    Code</walkthrough-editor-spotlight> from the status bar
*   Select
    <walkthrough-editor-spotlight spotlightId="cloud-code-run-on-cloud-run-emulator">Run
    on Cloud Run Emulator</walkthrough-editor-spotlight>
*   Your app will now be built and deployed again
*   Once your app is deployed, launch it with the link displayed in your
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel, near the text

    ```terminal
    http://localhost:8080
    Update successful
    ```

    You can open the web preview by clicking the <walkthrough-web-preview-icon/>
    button and selecting "Preview on port 8080".

## Conclusion

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

Congratulations! You've just created and run your first Anthos application.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

Here are some additional tutorials for using Cloud Code to develop your apps on
Google Cloud:

<walkthrough-tutorial-card url="https://ide.cloud.google.com/?walkthrough_tutorial_url=https%3A%2F%2Fwalkthroughs.googleusercontent.com%2Fcontent%2Fcloud_code_install_local_ide%2Fcloud_code_install_local_ide.md&show=ide" icon="LAUNCHER_SECTION" label="cloudSql">
**Cloud Code IDE extension** Install the Cloud Code extension on your local IDE
(VSCode/IntelliJ) </walkthrough-tutorial-card>

<walkthrough-tutorial-card url="https://ide.cloud.google.com/?walkthrough_tutorial_url=https%3A%2F%2Fwalkthroughs.googleusercontent.com%2Fcontent%2Fcloud_run_cloud_code_create_service%2Fcloud_run_cloud_code_create_service.md" icon="SERVERLESS_SECTION" label="cloudSql">
**Cloud Run** Create a serverless app from your browser
</walkthrough-tutorial-card>

