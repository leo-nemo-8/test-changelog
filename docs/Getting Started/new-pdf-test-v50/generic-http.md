---
title: Generic HTTP
excerpt: https://docs.clevertap.com/update/docs/generic-http-with-attachments
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

The Generic HTTP feature enables flexible integration with any third-party Email Service Provider (ESP) using the HTTP protocol. It supports sending email campaigns using custom payloads, headers, and dynamic fields. It also enables callback tracking for delivery success and failures, as well as user interactions such as clicks or unsubscribes.

# Set Up Email Provider in CleverTap

To configure any third-party ESP using the Generic HTTP provider in your CleverTap account, perform the following steps:

1. Go to _Settings_ > _Channels_ > _Email_.
2. Click **+ Provider** and select _Generic HTTP_ from the _Provider_ dropdown.
3. Enter the following provider details:

<Image align="center" alt="Provider Setup" border="{true}" caption="Provider Setup" src="[https://files.readme.io/b717e7c15eec34e63788f1928b5956c2a447899d506fc88c4b34f5f9e335708b-Provider_Setup.png](https://files.readme.io/b717e7c15eec34e63788f1928b5956c2a447899d506fc88c4b34f5f9e335708b-Provider_Setup.png)"/>

<Table>
  <thead>
    <tr>
      <th>Field</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nickname</td>
      <td>A unique name for identifying the configuration.</td>
    </tr>
    <tr>
      <td>Callback URL</td>
      <td>The Callback URL is used to receive bounce, delivery, and subscription information from the ESP to CleverTap. This is a read-only field that is auto-populated with CleverTap's callback URL. Ensure that you add this URL to your ESP dashboard settings to send callback data to CleverTap.</td>
    </tr>
    <tr>
      <td>Request Type</td>
      <td>Choose between `GET` or `POST`.<br /><br />**Note**: Attachments are only supported with POST requests.</td>
    </tr>
    <tr>
      <td>HTTP Endpoint</td>
      <td>API endpoint of the ESP. Must start with `http://` or `https://`.</td>
    </tr>
    <tr>
      <td>Authentication</td>
      <td>Choose _None_ or _Basic_. Provide credentials if using Basic authentication.</td>
    </tr>
    <tr>
      <td>Headers</td>
      <td>Add static or dynamic headers. For more information, refer to [Headers](/docs/generic-http#headers).</td>
    </tr>
    <tr>
      <td>Parameters</td>
      <td>Applicable for the POST method. Define the request payload in JSON format; supports both single and batch message delivery. For more information, refer to [Body Parameters](/docs/generic-http#body-parameters).</td>
    </tr>
    <tr>
      <td>Batch parameter</td>
      <td>- Applicable for the POST method.<br />- Enable sending up to 1,000 emails in a single API call.<br />- Before enabling this option, confirm the batch size supported by your ESP.<br /><br />For more information, refer to [Body Parameters](/docs/generic-http#body-parameters).</td>
    </tr>
    <tr>
      <td>Default From Address</td>
      <td>- This field is mandatory only if the feature is enabled for your account.<br />- Enter the default email address you want to use for sending your email campaigns.<br />- The email address will be pre-populated under the _From Email Address_ field as a default value when creating