# color-torch-browser
Browser Client for Color Torch

The browser client uses the [mqttSample](https://github.com/aws-samples/aws-iot-examples/tree/master/mqttSample) browser client from aws-samples/aws-iot-examples and modifies it to set color and state, as well as receive state back from AWS IoT.

**NOTE:** The bower modules are out-of-date and need to be updated. This will be addressed in a future release.

## Setup

1. [Create an AWS IAM user](https://console.aws.amazon.com/iam/home?region=us-east-1#/users$new) with the `AWSIoTFullAccess` policy attached.
   
   **NOTE:** Neither using an IAM user directly for a browser nor giving the user full permissions to AWS IoT is recommended. Please do not do either in a live environment on an internet-enabled site.

    1. Give the user a name (e.g. `iot-browser`), check **Programmatic access** and click **Next: Permissions**.
    1. Under **Set permissions**, click **Attach existing policies directly**.
    1. Search for `AWSIoTFullAccess`, check next to the policy name and click **Next: Tags**.
    1. Click **Next: Review** to skip the tags section.
    1. Click **Create user** to create the user.
    1. Save the *Access key ID* and *Secret access key* (click **Show** to focus on and highlight the secret access key). You will need these later.

1. Get the AWS IoT endpoint.

    ```bash
    aws iot describe-endpoint --endpoint-type "iot:Data-ATS"
    ```

1. Edit `src/app.js`, replacing the following values:

    * Line 78, replace `{{ endpoint }}` with the IoT endpoint.
    * Line 79, replace `{{ accessKey }}` with the *Access key ID* from the *iot-browser* IAM user.
    * Line 80, replace `{{ secretKey }}` with the *Secret access key* from the *iot-browser* IAM user.
    * Line 81, replace `{{ region }}` with your AWS region (e.g. `us-east-1`).
    * Line 109, replace `{{ thingName }}` with your IoT thing name (from the device).

1. Deploy the static website.

    Personally, I use Docker to deploy the site with nginx. Other methods will follow in future releases.

    From the working directory (*color-torch-browser* or *browser*), run:

    ```bash
    docker run -itd -v $(pwd):/usr/share/nginx/html -p 8080:80 --name color-torch-browser nginx:latest
    ```

    Then access the site at `http://localhost:8080/`.

1. Verify/enter the endpoint and click **Connect**. This establishes a connection to AWS IoT.

    The on/off button and color selector is visible, as well as an input/button to subscribe to the feed from the IoT thing. You can skip subscribing, but will not get desired states synchronizing back to your client (but sending commands still works).

1. (Optional) Verify/enter the thing name and click **Subscribe**.

