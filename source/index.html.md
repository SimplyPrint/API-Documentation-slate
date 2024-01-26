---
title: SimplyPrint API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL

toc_footers:
  - <a href='https://simplyprint.io' target='_blank'>SimplyPrint</a>
  - <a href='https://help.simplyprint.io' target='_blank'>Need help?</a>

includes:
  - printers
  - filament
  - files
  - queue
  - account
  - jobs
  - users
  - slicer
  - tags
  - permissions_scopes
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the SimplyPrint API
  - name: keywords
    content: SimplyPrint API,SimplyPrint Documentation
  - name: og:title
    content: SimplyPrint API Reference
  - name: og:description
    content: Documentation for the SimplyPrint API
  - name: og:image
    content: https://cdn.simplyprint.io/i/static/logo/png/2x/icon_gradient.png
  - name: og:url
    content: https://apidocs.simplyprint.io

---

# Getting Started

Welcome to the **SimplyPrint API**!

This documentation goes through authenticating and how to use the API.

## The base URL

The base URL for the SimplyPrint API is `https://api.simplyprint.io/{id}/`.

To use the base API at <https://api.simplyprint.io>, you will need to specify an id.

The id represents the unique identifier for the company that you are using the API key for. This id is used to access the specific functionality within the API that is associated with the company.

The endpoint represents the specific functionality that you want to access within the API. There are various endpoints available, each with its own specific purpose and functionality.

For example, if you want to access the `account/Test` endpoint for a company with an id of 123, you would use the following API endpoint:

<https://api.simplyprint.io/123/account/Test>

If you are part of an organization, you might need to ask your organization administrator to allow API access for your account for the organization.

<aside class="notice">
  <b>For the rest of this documentation, we will use the following variables:</b>

  <ol>
    <li>Replace <code>{base_url}</code> with your base api url</li>
    <li>Replace <code>{API_KEY}</code> with your API key</li>
  </ol>
</aside>

## Gaining access to the API

There are two ways to gain access to the SimplyPrint API:

1. [Using an API key](#using-an-api-key-recommended) (recommended)
   - This is the recommended method for most users. This method requires a SimplyPrint account and an API key.
2. [Using OAuth2](#using-oauth2)
   - This method is only available for approved integrations. You can use OAuth2 to access SimplyPrint API endpoints on behalf of a user.

## Using an API key (recommended)

To get an API key you'll need a SimplyPrint account that is either a member of an organization, or have at least the [SimplyPrint Pro plan](https://simplyprint.io/pricing).

To create your own API key you first need a SimplyPrint account, you can go to your [account settings](https://simplyprint.io/panel/user_settings/api) and create a new API key.

### Using the API key

```shell
curl {base_url}/account/Test \
  --header 'X-API-KEY: {API_KEY}'
```

> Success return:

```json
{
  "status": true,
  "message": "Your API key is valid!"
}
```

> Error return in case of an invalid or missing API key:

```json
{
  "status": false,
  "message": "No API key provided, or not logged in"
}
```

> Error return in case of missing permissions for the organization:

```json
{
  "status": false,
  "message": "You don't have access to this account"
}
```

To verify that your API key is valid and that you have access to the desired organization, you can make a request to the `/account/Test` endpoint.

To make any request to the SimplyPrint API, you will need to include your API key in the request header. You can do this by including the `X-API-KEY` header in your request. On the right side of this page, you can see an example of how to make a request to the `/account/Test` endpoint using cURL.

If you are unable to successfully make a request to the `/account/Test` endpoint using the provided example, there may be a few possible issues:

- Your API key may be invalid or missing. Make sure that you have a valid API key and that you are including it in the request header as shown in the example.
- You may not have access to the organization specified in the `id` parameter. Make sure that you are using the correct `id` for the organization that you have access to.
- There may be a problem with the base API URL or endpoint. Double check that you are using the correct base API URL and endpoint in your request.

If you are unable to resolve the issue after troubleshooting these potential issues, you may need to contact your organization administrator for further assistance. Otherwise, feel free to contact us via our [helpdesk](https://help.simplyprint.io/).

## Using OAuth2

SimplyPrint supports OAuth2 logins for all users, regardless of their subscription, for approved integrations such as the [Cura slicer integration](https://simplyprint.io/integrations/cura).

The OAuth2 method can be used to link a user's SimplyPrint account with a third party platform/software, and use the SimplyPrint API on behalf of the user.

To obtain OAuth2 access, you must be added as an OAuth2 provider by SimplyPrint. To request this, please fill out the [OAuth2 Client Request Form](https://forms.gle/rC9AzWwNtwMmWXE68).

Once you have been added as an OAuth2 provider, you can use the following documentation to implement OAuth2 in your application.

### Authorizing your application

For users to grant your application access to their SimplyPrint account, you must first redirect them to the SimplyPrint OAuth2 authorization page. You can do this by redirecting the user to the following URL:

`https://api.simplyprint.io/oauth2/authorize?client_id={client_id}&redirect_uri={redirect_uri}&response_type=code&scope={scope}`

The following parameters can be supplied as query parameters in the URL:

| Parameter       | Type   | Required | Description                                                                                         |
| --------------- | ------ | -------- | --------------------------------------------------------------------------------------------------- |
| `client_id`     | string | yes      | The client ID of your OAuth2 application                                                            |
| `redirect_uri`  | string | yes      | The redirect URI of your OAuth2 application                                                         |
| `scope`         | string | yes      | The [scope](#oauth2-scopes) of the OAuth2 access token. This can be a `+`-separated list of scopes. |
| `response_type` | string | yes      | This must be set to `code`                                                                          |
| `state`         | string | no       | The value of this parameter will be returned to your application when the user is redirected back   |

Once the user has granted access to your application, they will be redirected to `{redirect_uri}?code={code}&state={state}`. The `code` parameter is the authorization code that you will use to obtain an access token. The `state` parameter will only be returned if you specified a `state` parameter in the authorization URL.

### Obtaining an access token

> Obtaining an access token

```shell
curl https://api.simplyprint.io/oauth2/Token \
  -X POST \
  -H 'accept: application/json' \
  -H 'X-API-KEY: {API_KEY}'
```

> Request body

```json
{
  "grant_type": "authorization_code",
  "client_id": "{client_id}",
  "client_secret": "{client_secret}",
  "code": "{code}",
  "redirect_uri": "{redirect_uri}"
}
```

> Success response

```json
{
  "token_type": "Bearer",
  "expires_in": 3600,
  "access_token": "{access_token}",
  "refresh_token": "{refresh_token}"
}
```

Now that you have a `code` parameter, you can use it to obtain an access token. To do this, you must make a `POST` request to the following URL:

`https://api.simplyprint.io/oauth2/Token`

This will return an access token that you can use to make requests to the SimplyPrint API on behalf of the user.

This access token will expire after 1 hour. Once it expires, you can use the `refresh_token` parameter to obtain a new access token. To do this, you must make a `POST` request to the same URL as above, but with the following request parameters:

| Parameter       | Type   | Description                                                            |
| --------------- | ------ | ---------------------------------------------------------------------- |
| `grant_type`    | string | This must be set to `refresh_token`                                    |
| `client_id`     | string | The client ID of your application                                      |
| `client_secret` | string | The client secret of your application                                  |
| `refresh_token` | string | The refresh token that was returned when you obtained the access token |

To refresh your access token, you can make a `POST` request to the same URL as above, but with the following request parameters:

| Parameter       | Type   | Description                                                            |
| --------------- | ------ | ---------------------------------------------------------------------- |
| `grant_type`    | string | This must be set to `refresh_token`                                    |
| `client_id`     | string | The client ID of your application                                      |
| `client_secret` | string | The client secret of your application                                  |
| `refresh_token` | string | The refresh token that was returned when you obtained the access token |

This will return a new access token that you can use to make requests to the SimplyPrint API on behalf of the user.

### Testing your access token

> Testing your access token

```shell
curl https://api.simplyprint.io/oauth2/TokenInfo \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer {access_token}'
```

> Success response

```json
{
  "status": true,
  "message": null,
  "user": {
    "id": 112,
    "name": "John Doe",
    "email": "john@doe.com",
  },
  "company": {
    "id": 123,
    "name": "My Company"
  },
  "scopes": [
    ...
  ],
  "expires_at": 1706292803
}
```

To test your access token, you can make a `GET` request to the following URL:

`https://api.simplyprint.io/oauth2/TokenInfo`

This endpoint returns the following information:

| Parameter      | Type    | Description                                                                                 |
| -------------- | ------- | ------------------------------------------------------------------------------------------- |
| `status`       | boolean | True if the request was successful.                                                         |
| `message`      | string  | Error message if `status` is false.                                                         |
| `user`         | object  | User object.                                                                                |
| `user.id`      | integer | User ID.                                                                                    |
| `user.name`    | string  | User name. This is only returned if the `user.read` scope is included in the access token.  |
| `user.email`   | string  | User email. This is only returned if the `user.read` scope is included in the access token. |
| `company`      | object  | Company object.                                                                             |
| `company.id`   | integer | Company ID.                                                                                 |
| `company.name` | string  | Company name.                                                                               |
| `scopes`       | array   | Array of scopes that are included in the access token.                                      |
| `expires_at`   | integer | The unix timestamp of when the access token expires.                                        |

The company id is especially important, as you will need to use this in the base URL for all requests to the SimplyPrint API.
