# Users

A user in Todoist is a JSON object. The dates will be in the UTC timezone. Typically a user object will have the following properties:

> An example of a user object:

```shell
{
  "start_page": "overdue, 7 days",
  "is_premium": false,
  "sort_order": 0,
  "full_name": "Example User",
  "has_push_reminders": false,
  "timezone": "Europe\/Athens",
  "id": 1855589,
  "next_week": 1,
  "completed_count": 20,
  "tz_offset": ["+03:00", 3, 0, 1],
  "email": "me@xample.com",
  "karma": 684.0,
  "start_day": 1,
  "date_format": 0,
  "inbox_project": 128501411,
  "time_format": 0,
  "image_id": null,
  "beta": 0,
  "karma_trend": "-",
  "business_account_id": null,
  "mobile_number": null,
  "mobile_host": null,
  "is_dummy": 0,
  "premium_until": null,
  "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
  "api_token": "0123456789abcdef0123456789abcdef01234567",
  "is_biz_admin": false,
  "default_reminder": null
}
```

```python
{
  'beta': 0,
  'business_account_id': None,
  'completed_count': 20,
  'date_format': 0,
  'default_reminder': None,
  'email': 'me@exampe.com',
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
  'mobile_host': None,
  'mobile_number': None,
  'next_week': 1,
  'premium_until': None,
  'sort_order': 0,
  'start_day': 1,
  'start_page': 'overdue, 7 days',
  'time_format': 0,
  'timezone': 'Europe/Athens',
  'api_token': '0123456789abcdef0123456789abcdef01234567',
  'tz_offset': ['+03:00', 3, 0, 1]
}
```
### Properties

Property | Description
-------- | -----------
id| User's unique id.
api_token | User's token (that should be used to call the other API methods).
email | User's email.
full_name | User's real name.
start_page | User's default view on Todoist. The start page can be one of the following: `_info_page` for the info page, `_blank` for a blank page, `_project_<PROJECT_ID>` for project with id `<PROJECT_ID>`, and `<ANY_QUERY>` to query after anything.
timezone | User's timezone in a string.
tz_offset | User's timezone offset `[GMT_STRING, HOURS, MINUTES, IS_DAYLIGHT_SAVINGS_TIME]`.
time_format | If it's `0` then show time as `13:00`, else show time as `1:00pm`.
date_format | If it's `0` then show dates as `DD-MM-YYYY`, else show dates as `MM-DD-YYYY`.
sort_order | If it's `0` then show `Oldest dates first` when viewing projects. Else `Oldest dates last`.
mobile_number | User's mobile number.
mobile_host | User's mobile host.
premium_until | When does the user's Premium subscription expire?
default_reminder | What is the default reminder for the user? Reminders are only possible for Premium users. The default reminder can be one of the following:  `email`, `mobile`, `push`, and `no_default`.
auto_reminder | How many minutes before should the default reminder be set? It can be `-1, 0, 10, 30, 60, 120`, e.g. 10 minutes before the due date. `-1` is set to mean that no default reminder will be added

## Register a new user

> And example of registering a new user:


```shell
$ curl https://todoist.com/API/v6/register \
    -d email=me@example.com \
    -d full_name=Example\ User \
    -d password=secret
{
  "start_day": 7,
  "start_page": "overdue, 7 days",
  "date_format": 1,
  "last_used_ip": "10.20.30.40",
  "api_token": "0123456789abcdef0123456789abcdef01234567",
  "karma_trend": "-",
  "inbox_project": 128501411,
  "time_format": 1,
  "image_id": null,
  "beta": 0,
  "sort_order": 0,
  "business_account_id": null,
  "full_name": "Example User",
  "mobile_number": null,
  "shard_id": 2,
  "timezone": "UTC",
  "is_premium": false,
  "mobile_host": null,
  "id": 1855589,
  "has_push_reminders": false,
  "is_dummy": 0,
  "premium_until": null,
  "team_inbox": null,
  "next_week": 1,
  "token": "0123456789abcdef0123456789abcdef01234567",
  "tz_offset": ["+00:00", 0, 0, 0],
  "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
  "seq_no": 2180270834,
  "karma": 0.0,
  "is_biz_admin": false,
  "default_reminder": null,
  "email": "me@example.com"}
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI()
>>> api.register('me@example.com', 'Example User', 'secret')
{
  'start_page': 'overdue, 7 days',
  'join_date': 'Wed 30 Apr 2014 13:24:38 +0000',
  'last_used_ip': '10.20.30.40',
  'is_premium': False,
  'sort_order': 0,
  'full_name': 'Example User',
  'api_token': '0123456789abcdef0123456789abcdef01234567',
  'shard_id': 2,
  'has_push_reminders': False,
  'id': 1855589,
  'team_inbox': None,
  'next_week': 1,
  'tz_offset': ['+00:00', 0, 0, 0],
  'timezone': 'UTC',
  'email': 'me@example.com',
  'start_day': 7,
  'is_dummy': 0,
  'inbox_project': 128501411,
  'time_format': 1,
  'image_id': None,
  'beta': 0,
  'premium_until': None,
  'business_account_id': None,
  'mobile_number': None,
  'mobile_host': None,
  'date_format': 1,
  'karma_trend': '-',
  'token': '0123456789abcdef0123456789abcdef01234567',
  'seq_no': 2180270834,
  'karma': 0.0,
  'is_biz_admin': False,
  'default_reminder': None
}

```

Register a new user.

### Required parameters

Parameter | Description
--------- | -----------
email | User's email.
full_name | User's full name.
password | User's password, should be at least 5 characters long.

### Optional parameters

Parameter | Description
--------- | -----------
lang | User's language. Can be `de`, `fr`, `ja`, `pl`, `pt_BR`, `zh_CN`, `es`, `hi`, `ko`, `pt`, `ru`, `zh_TW`.
timezone | User's timezone. As default we use the user's IP address to determine the timezone.

## Delete an existing user

> An example of deleting an existing user:

```shell
$ curl https://todoist.com/API/v6/delete_user \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d current_password=secret
"ok"
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.delete_user('secret')
ok
```

Delete an existing user.

### Required parameters

Parameter | Description
--------- | -----------
token | User's token.
current_password | User's current password.

### Optional parameters

Parameter | Description
--------- | -----------
reason_for_delete | Reason for deletion (used for feedback).


## Update user's properties


> An example of updating the user's properties:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "user_update", "uuid": "52f83009-7e27-4b9f-9943-1c5e3d1e6889", "args": {"time_format": 0}}]'
{ ...
  "SyncStatus": {"52f83009-7e27-4b9f-9943-1c5e3d1e6889": "ok"},
  ... }

```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.user.update(time_format=0)
```

Update the details of the user.

### Optional parameters

Parameter | Description
--------- | -----------
email | User's email.
full_name | User's full name.
password | User's password, which should be at least 5 characters long.
timezone | User's timezone.
date_format | How should dates be formatted? If `0` `DD-MM-YYYY` will be used, if `1` `MM-DD-YYYY` will be used.
time_format | How should times be formatted? If `0` `13:00` will be used, if `1` `1:00pm` will be used.
start_day | First day of week. `1` for Monday, `7` for Sunday.
next_week | When postponing what day should we choose? `1` for Monday, `7` for Sunday.
start_page | Can be one of following values: `_blank` to show blank page, `_info_page` to show info page `_project_<PROJECT_ID>` where `<PROJECT_ID>` is the id of the project to show, `<ANY_QUERY>` to query after anything (for example `tod,tom,!!1`).
default_reminder | Can be one of the following values: `email` to send reminders by email, `mobile` to send reminders to mobile devices via SMS, `push` to send reminders to smart devices using push notifications (one of Android or iOS official client must be installed on the client side to receive these notifications), `no_default` to turn off sending default reminders.

## Update karma goals

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "update_goals", "uuid": "b9bbeaf8-9db6-452a-a843-a192f1542892", "args": {"vacation_mode": 1}}]'
{ ...
  "SyncStatus": {"b9bbeaf8-9db6-452a-a843-a192f1542892": "ok"},
  ... }

```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.user.update_goals(vacation_mode=1)
```

Update the karma goals of the user.

### Optional parameters

Parameter | Description
--------- | -----------
daily_goal | The target number of tasks to complete per day.
weekly_goal | The target number of tasks to complete per week.
ignore_days | A list with the days of the week to ignore.  `1` for Monday, `7` for Sunday.
vacation_mode | Marks the user as being on vacation.
karma_disabled | Disables the karma and goals measuring alltogether.
