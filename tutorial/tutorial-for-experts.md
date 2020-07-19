---
description: >-
  If you are proficient in developing and deploying vision apps, follow this
  expedited tutorial.
---

# Tutorial for Experts

## Expose your vision app as an API.

Set up an API server that runs your vision app when it receives a POST request defined as following:

{% api-method method="post" host="http://{YOUR\_SERVER\_ADDRESS}/" path=" " %}
{% api-method-summary %}
Request
{% endapi-method-summary %}

{% api-method-description %}
When your server receives a POST request, you can read image from req.body.image
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="vision" type="object" required=true %}
Image encoded in base64
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Return an HTML string that Interface client will render for the users. You can use custom HTMl style or attach media in base64. 
{% endapi-method-response-example-description %}

```
"""
<html>
Text.
<div style=\"color:black\">Text with custom style.</div>
<div><img src="{image encoded in base64}"/></div>
</html>
"""
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Below is a simple Python server that reads image from the request and passes the image to an `app` function.

```text
from flask import Flask, request, Response, redirect
from base64 import b64decode
from imageio import imread
import cv2
import io

from app import app

server = Flask(__name__)

@server.route('/', methods=['POST', 'GET'])
def main():
    req = request.form.to_dict()
    image_string = b64decode(req["vision"].partition('base64,')[2])
    image = cv2.cvtColor(imread(io.BytesIO(image_string)), cv2.COLOR_RGB2BGR)
    return app(image)

if __name__ == '__main__':
    server.run(host='0.0.0.0', port=8888, debug=True)
```

## Register your vision app on Interface.

Send a POST request to Interface platform registering the API endpoint to your vision app.

{% api-method method="post" host="https://interface.product.ceo/register" path=" " %}
{% api-method-summary %}
Register
{% endapi-method-summary %}

{% api-method-description %}
Send a POST request to https://interface.product.ceo/register with a JSON object that contains the URL that points to your vision app in the request body. Make sure the URL you send includes the protocol \("https://\). You will receive a link in the form https://interface.product.ceo/{id} as a response. Interface platform will forward user requests from that returned URL to the URL to your vision app. 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="url" type="string" required=true %}
https://example.product.ceo/
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The response contains a link to your vision app on Interface platform.
{% endapi-method-response-example-description %}

```
https://interface.product.ceo/j8u7n9w1o2n3
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Share your vision app with your users.

> Hey, check out my vision app at https://interface.product.ceo/j8u7n9w1o2n3

Copy the link. Send it to your users. They will open the link, take a photo, and see the result returned by your vision app. It's that simple.

If the vision app id is empty or invalid, Interface will default to an example vision app that returns a horizontally flipped grayscale version of the image that the user sent.

