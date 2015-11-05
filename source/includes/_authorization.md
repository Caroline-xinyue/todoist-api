# Authorization

In order to make authorized calls to Todoist APIs, your application must first obtain an access token from the users. This section describes the different ways of obtaining such a token.

Note that we encourage your application to use the [OAuth](http://en.wikipedia.org/wiki/OAuth) protocol to obtain the access token from the user, as the other authentication methods (`login` and `login_with_google`) are scheduled for deprecation.

## OAuth

External applications could obtain a user authorized API token via the OAuth2 protocol. Before getting started, developers need to create their applications in App Management Console and configure a valid OAuth redirect URL. A registered Todoist application is assigned a unique Client ID and Client Secret which are needed for the OAuth2 flow.

This procedure is comprised of 3 steps, which will be described below.

### Step 1: The authorization request

> An example of redirecting a user to the authorization URL:

```shell
$ curl "https://todoist.com/oauth/authorize" \
    -d "client_id=0123456789abcdef" \
    -d "scope=data:read,data:delete" \
    -d "state=secretstring"
```

Redirect users to the authorization URL at the endpoint `https://todoist.com/oauth/authorize`, with the specified request parameters.

Here follow the required parameters:

Name | Description
---- | -----------
client_id | The unique Client ID of the Todoist application that you registered.
scope | A comma separated list of permissions which you would like the users to grant to your application. See below a table with more details about this.
state | A unique and unguessable string. It is used to protect you against cross-site request forgery attacks.


Here are the scope parameters mentioned before:

Name | Description
---- | -----------
task:add | Grants permission to add tasks to the Inbox project (The application cannot read tasks data).  
data:read | Grants read-only access to application data, including tasks, projects, labels, and filters. 
data:read_write | Grants read and write access to application data, including tasks, projects, labels, and filters. This scope include `task:add` and `data:read` scopes.
data:delete | Grants permission to delete application data, including tasks, labels, and filters. 
project:delete | Grants permission to delete projects.

And here are some common errors that you may encountered:

Error | Description
----- | -----------
User Rejected Authorization Request | When the user denies your authorization request, Todoist will redirect the user to the configured redirect URI with `error` paramete: `http://example.com?error=access_denied`.
Redirect URI Not Configured | This JSON error will be returned to the requester (your user's browser) if redirect URI is not configured in the App Management Console.
Invalid Application Status | When your application exceeds the maximum token limit or when your application is being suspended due to abuse, Todoist will redirect the user to the configured redirect URI with the `error` parameter: `http://example.com?error=invalid_application_status`.
Invalid Scope | When the `scope` parameter is invalid, Todoist will redirect the user to the configured redirect URI with `error` parameter: `http://example.com?error=invalid_scope`.

### Step 2: The redirection to your application site

When the user grants your authorization request , the user will be redirected to the redirect URL configured in your application setting. The redirect request will come with two query parameters attached: `code` and `state`. 

The `code` parameter contains the authorization code that you will use to exchange for an access token. The `state` parameter should match the `state` parameter that you supplied in the previous step.  If the `state` is unmatched, your request has been compromised by other parties, and the process should be aborted.

### Step 3: The token exchange

> An example of exchanging the token:

```shell
$ curl "https://todoist.com/oauth/access_token" \
    -d "client_id=0123456789abcdef" \
    -d "client_secret=secret" \
    -d "code=abcdef" \
    -d "redirect_uri=https://example.com"
```

> On success, Todoist returns HTTP 200 with token in JSON object format:

```json
{
  "access_token": "0123456789abcdef0123456789abcdef01234567", 
  "token_type": "Bearer"
}
```

Once you have the authorization `code`, you can exchange it for the access token at the endpoint `POST https://todoist.com/oauth/access_token`.

Here follow the required parameters:

Name | Description
---- | -----------
client_id | The unique Client ID of the Todoist application that you registered.
client_secret | The unique Client Secret of the Todoist application that you registered.
code | The unique string code which you obtained in the previous step.

And here are some common errors that you may encountered (all the error response will be in JSON format):

Error | Description
----- | -----------
Bad Authorization Code Error | Occurs when the `code` parameter does not match the code that is given in the redirect request: `{"error": "bad_authorization_code"}`
Incorrect Client Credentials Error | Occurs when the `client_id` or `client_secret` parameters are incorrect: `{"error": "incorrect_application_credentials"}`

## Login with password

> On success, an HTTP 200 OK with a JSON object with user data is returned:

```shell
$ curl "https://todoist.com/API/v6/login" \
    -d "email=me@example.com" \
    -d "password=secret"
```


```python
>>> import todoist
>>> api = todoist.TodoistAPI()
>>> api.login('me@example.com', 'secret')
```


> On success, Todoist returns HTTP 200 with token in JSON object format:

```json
{
  "start_page": "overdue, 7 days",
  "date_format": 0,
  "last_used_ip": "10.20.30.40",
  "api_token": "0123456789abcdef0123456789abcdef01234567",
  "karma_trend": "-",
  "inbox_project": 128501411,
  "time_format": 0,
  "image_id": null,
  "beta": 0,
  "sort_order": 0,
  "business_account_id": null,
  "full_name": "Example User",
  "mobile_number": null,
  "shard_id": 2,
  "timezone": "Europe/Athens",
  "is_premium": false,
  "mobile_host": null,
  "id": 1855589,
  "has_push_reminders": false,
  "is_dummy": 0,
  "premium_until": null,
  "team_inbox": null,
  "next_week": 1,
  "token": "0123456789abcdef0123456789abcdef01234567",
  "tz_offset": ["+03:00", 3, 0, 1],
  "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
  "seq_no": 2180270834,
  "karma": 684.0,
  "is_biz_admin": false,
  "default_reminder": null,
  "email": "me@example.com"
}
```


Login user into Todoist to get a token, which is necessary to do any communication with the server.

### Required parameters

Parameter | Description
--------- | -----------
email | User's email.
password | User's password.

## Login with Google

> On success, an HTTP 200 OK with a JSON object with user data is returned:

```shell
$ curl "https://todoist.com/API/v6/login_with_google" \
    -d "email=me@example.com" \
    -d "oauth2_token=01234567-89ab-cdef-0123-456789abcdef"

{
  "start_page": "overdue, 7 days",
  "date_format": 0,
  "last_used_ip": "10.20.30.40",
  "api_token": "0123456789abcdef0123456789abcdef01234567",
  "karma_trend": "-",
  "inbox_project": 128501411,
  "time_format": 0,
  "image_id": null,
  "beta": 0,
  "sort_order": 0,
  "business_account_id": null,
  "full_name": "Example User",
  "mobile_number": null,
  "shard_id": 2,
  "timezone": "Europe/Athens",
  "is_premium": false,
  "mobile_host": null,
  "id": 1855589,
  "has_push_reminders": false,
  "is_dummy": 0,
  "premium_until": null,
  "team_inbox": null,
  "next_week": 1,
  "token": "0123456789abcdef0123456789abcdef01234567",
  "tz_offset": ["+03:00", 3, 0, 1],
  "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
  "seq_no": 2180270834,
  "karma": 684.0,
  "is_biz_admin": false,
  "default_reminder": null,
  "email": "me@example.com"
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI()
>>> api.login_with_google('me@example.com', '01234567-89ab-cdef-0123-456789abcdef')
{
  'api_token': '0123456789abcdef0123456789abcdef01234567',
  'beta': 0,
  'business_account_id': None,
  'date_format': 0,
  'default_reminder': None,
  'email': 'me@example.com',
  'full_name': 'Example User',
  'has_push_reminders': False,
  'id': 1855589,
  'image_id': None,
  'inbox_project': 128501411,
  'is_biz_admin': False,
  'is_dummy': 0,
  'is_premium': False,
  'join_date': 'Wed 30 Apr 2014 13:24:38 +0000',
  'karma': 684.0,
  'karma_trend': '-',
  'last_used_ip': '10.20.30.40',
  'mobile_host': None,
  'mobile_number': None,
  'next_week': 1,
  'premium_until': None,
  'seq_no': 2180270834,
  'shard_id': 2,
  'sort_order': 0,
  'start_day': 1,
  'start_page': 'overdue, 7 days',
  'team_inbox': None,
  'time_format': 0,
  'timezone': 'Europe/Athens',
  'token': '0123456789abcdef0123456789abcdef01234567',
  'tz_offset': ['+03:00', 3, 0, 1]
}
```

Login user into Todoist with Google account.

### Implementation notes

While authenticating, application exchanges their users' OAuth 2.0 access tokens to tokens provided by Todoist. OAuth 2.0 access token must be issued for the scope `oauth2:https://www.googleapis.com/auth/userinfo.email`

It doesn't matter how you receive the access token. Android applications can use [AccountManager](http://developer.android.com/reference/android/accounts/AccountManager.html) to get it done. Other developers should consider using [generic authentication instructions](https://developers.google.com/accounts/docs/OAuth2Login).

### Required parameters

Parameter | Description
--------- | -----------
email | User's email.
oauth2_token | OAuth 2.0 access token, received by application from user.

### Optional parameters

Parameter | Description
--------- | -----------
auto_signup | If set to `1` and user with this email and Google Account doesn't exist yet, then automatically registers a new account.
full_name | User's full name if user is about to be registered. If not set, a user email nickname will be used.
timezone | User's timezone if user is about to be registered. If not set, we guess the timezone from the client's IP address. In case of failure, "UTC" timezone will be set up for a newly created account.
lang | User's language. Can be `de`, `fr`, `ja`, `pl`, `pt_BR`, `zh_CN`, `es`, `hi`, `ko`, `pt`, `ru`, `zh_TW`

## Token from Settings

You can also obtain your personal access token directly from the Todoist web application Settings: 

`Settings` -> `Todoist Settings` -> `Account` -> `API token`
