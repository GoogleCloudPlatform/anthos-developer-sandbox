# Anthos Developer Sandbox

<walkthrough-disable-features toc></walkthrough-disable-features>

## Introduction

This tutorial gives you some experience with Anthos development practices
such as continuous/iterative development, local builds, and local deployments.

In this tutorial, you learn how to perform the following tasks: 

1.  Run this app on a local Kubernetes cluster with minikube.
2.  Use the Cloud Build local builder to run tests locally,
3.  Use Cloud Code to quickly test, debug, and iterate on changes.
4.  Build the application with Cloud Buildpacks.
5.  Deploy the app to the Cloud Run Local Emulator.

## Run your app in a local cluster

The sample application is already available to you. You run it on `minikube`,
a local Kubernetes cluster available by default in Cloud Shell, which gives you the
same experience as deploying to Kubernetes, without needing infrastructure.

1.  In your
    <walkthrough-editor-spotlight spotlightId="menu-terminal-new-terminal">terminal</walkthrough-editor-spotlight>,
    run the following command:

    ```bash
    minikube start -p cloud-run-dev-internal
    ```

*   If prompted, authorize Cloud Shell to make Google Cloud API calls.

*   When your local cluster in Cloud Shell is set up, the following
    message is displayed:

    ```terminal
    Done! kubectl is now configured to use
    "cloud-run-dev-internal" by default
    ```



*   Click
    <walkthrough-editor-spotlight spotlightId="cloud-code-status-bar">Cloud
    Code</walkthrough-editor-spotlight> in the status bar.
*   Select <walkthrough-editor-spotlight spotlightId="cloud-code-run-on-k8s">Run
    on Kubernetes</walkthrough-editor-spotlight>, and confirm that you want to use
    the "cloud-run-dev-internal" context.
*   An
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel will appear after a few seconds and display the progress as your app
    is built/deployed.
*   When your app is built, to launch it, click the Web
    Preview link displayed in your
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel, near the text:

    ```terminal
    Forwarded URL from service
    anthos-sandbox-sample-application-external:
    ```

Congratulations! You've just run your first app on Kubernetes using
[Cloud Code](https://cloud.google.com/code). 

## Use the Cloud Build local builder to run tests locally

Now run the tests for your application locally with the
Cloud Build local builder (`cloud-build-local`), which allows you to run the
same build locally that your continuous integration suite would run.

*   In your
    <walkthrough-editor-spotlight spotlightId="menu-terminal-new-terminal">terminal</walkthrough-editor-spotlight>,
    to run the tests
    (<walkthrough-editor-open-file filePath="tests/test_application.py">test_application.py</walkthrough-editor-open-file>), run the following command:

    ```bash
    cloud-build-local --dryrun=false .
    ```

    **Note** This command will produce some warnings, which can safely be
    ignored.

*   A message indicates that the tests have failed:

    ```terminal
    ## FAIL: test_my_app (tests.test_application.TestMyApplication)

    Traceback (most recent call last): File
    "/workspace/tests/test_application.py", line 26, in test_my_app assert
    response.data == b"Hello Anthos" AssertionError

    ----------------------------------------------------------------------------

    Ran 1 test in 0.021s

    FAILED (failures=1)
    ```

The expected result was the application saying "Hello Anthos" instead of
"Hello World." You will fix that in the next procedure.

## Edit and recompile your app

Cloud Code enables iterative development, so your deployed app updates as you
write code, with no need to redeploy after every change. Your application is
composed of the following:

*   A basic web app,
    (<walkthrough-editor-open-file filePath="src/main.py">main.py</walkthrough-editor-open-file>),
    that returns "Hello, world!" to all received requests
*   A load balancer, "anthos-sandbox-sample-application-external"
    [Kubernetes Service](https://kubernetes.io/docs/concepts/services-networking/service/)
    (<walkthrough-editor-open-file filePath="kubernetes-manifests/hello.service.yaml">hello.service.yaml</walkthrough-editor-open-file>),
    that exposes your app to the internet

To modify and automatically rebuild your app:

1.   Change
    <walkthrough-editor-select-line filePath="src/main.py" startLine="9" endLine="9" startCharacterOffset="19" endCharacterOffset="24">main.py</walkthrough-editor-select-line>
    to print "Hello, Anthos!" The file will be saved automatically.
*   The
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel indicates that your app automatically starts rebuilding.
2.   To view the updated app text,
    click the link in your
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel or return to your open tab and refresh it.

## Build the application with Cloud Buildpacks

[Google Cloud Buildpacks](https://cloud.google.com/blog/products/containers-kubernetes/google-cloud-now-supports-buildpacks)
allow you to create production-ready container images, which are ready to be deployed anywhere container
images can be deployed, directly from source code
without having to use a Dockerfile.

First, you'll re-run the tests.

1.   In your
    <walkthrough-editor-spotlight spotlightId="menu-terminal-new-terminal">terminal</walkthrough-editor-spotlight>,
    run the following command:

    ```bash
    cloud-build-local --dryrun=false .
    ```

    **Note** This command will produce some warnings, which can safely be
    ignored.

*   The tests are now passing:

    ```terminal
    Ran 1 test in 0.020s

    OK
    ```

*   As part of the build process, the `cloud-build-local` tool has also used
    Google Cloud Buildpacks to produce a container image for your application.

2.   To verify that your image has been built, run the following command
    and view the details under the `LOCAL` output:

    ```bash
    pack inspect-image anthos-sandbox-sample-application
    ```


## Deploy the app to the Cloud Run Emulator

Now that you've built an image with Cloud Buildpacks, you can deploy it
directly to Cloud Run to take advantage of its ability to scale up whenever
traffic increases and scale to zero when it's not being used, thus eliminating any
idle costs.

You can get the same experience as deploying to Cloud Run by redeploying your app
locally on the Cloud Run Emulator:

1.   Launch
    <walkthrough-editor-spotlight spotlightId="cloud-code-status-bar">Cloud
    Code</walkthrough-editor-spotlight> from the status bar.
2.   Select
    <walkthrough-editor-spotlight spotlightId="cloud-code-run-on-cloud-run-emulator">Run
    on Cloud Run Emulator</walkthrough-editor-spotlight>.
    Your app will now be built and deployed again.
   
3.   After your app is deployed, to launch it, click the link displayed in your
    <walkthrough-editor-spotlight spotlightId="output">Output</walkthrough-editor-spotlight>
    panel.


 4.   To open the web preview, click the <walkthrough-web-preview-icon/>
    button, and select __Preview on port 8080__.

## Conclusion

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

Congratulations! You've just created and run your first Anthos application.

<walkthrough-inline-feedback></walkthrough-inline-feedback>

Here are some additional tutorials for using Cloud Code to develop your apps on
Google Cloud:

<walkthrough-tutorial-card id="cloud_code_install_local_ide" icon="LAUNCHER_SECTION">
**Cloud Code IDE extension** Install the Cloud Code extension on your local IDE
(VSCode/IntelliJ) </walkthrough-tutorial-card>

<walkthrough-tutorial-card id="cloud_run_cloud_code_create_service" icon="SERVERLESS_SECTION">
**Cloud Run** Create a serverless app from your browser
</walkthrough-tutorial-card>
