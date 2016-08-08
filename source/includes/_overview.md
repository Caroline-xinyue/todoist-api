# Overview

This is the official Todoist API documentation, a reference to the functionality our public API provides, with detailed description of each API endpoint, its parameters, and examples.

**Note that documentation for the older [Todoist API v6](https://developer.todoist.com/v6/) is still available, but we strongly recommend you use or migrate your code to the current version, as the v6 may be deprecated in the near future. You can find a changelog and migration guide at the end of this section.**


## Summary of contents

In the `Getting started` section we will try to present to you the Todoist API, in the simplest possible way, by using real examples, based on common tasks that many users want to accomplish.

After reading this section you should continue with the `Authentication`, in order to learn all the details on the best way to authenticate to our server.

The most important section is the `Sync` section, and you should read it next, where the way that the API works is explained.

The `Changelog` section is useful for those migrating from older versions of our API.

The rest sections are the reference documentation of the different Todoist objects and endpoints, and you can continue reading them in the order you need them.


## Client libraries and tools

Currently there is official support in the form of a Python library, but soon more languages might follow.  There's also a repository with some API tools that might be useful to users.

### Python

> You can install todoist python library via pip:

```
$ pip install todoist-python
```

You can find the [Python library source code](https://github.com/doist/todoist-python) at its repository at Github.

You can also read the [Python library documentation](http://todoist-python.readthedocs.org/en/latest/) online.

Finally, there's also a [PyPI package](https://pypi.python.org/pypi/todoist-python) ready for you to install.

### API tools

You can find the [API tools](https://github.com/doist/todoist-api-tools) at its repository at Github.

Examples of usage of the API tools can be found later on in this documentation.


## Migration guide

The new v7 Todoist API is still based on the original Todoist Sync API (as was the case for the v6 API), so the differences between the two APIs aren't as extended as those between the v5 and v6 APIs.

In this section we document all the changes between the two different versions of our API, in order to make it easier to upgrade your client code.

The main difference between the two APIs is that the `seq_no`, ie. the sequence number which was basically an always increasing number, is not used anymore to implement the incremental sync functionality, but instead the `sync_token` has been introduced, a string hash value.  So in order to do the initial full sync instead of `seq_no=0`, one needs to send `sync_token=*`.  Other than than, the last known value of `sync_token` should be used in order to an incremental sync, similar to what was done before with the `seq_no`.

Here follows a list of various minor changes from the previous API version:

* The `sync_token` value is not returned by the `sync` call anymore, but the `sync_token` and the `full_sync` variables are returned instead.
* The `sync` call now returns all objects in underscore naming convention, instead of CamelCase, so the `Collaborators`, `CollaboratorStates`, `DayOrders`, `DayOrdersTimestamp`, `Filters`, `Items`, `Labels`, `LiveNotifications`, `LiveNotificationsLastRead`, `Notes`, `Projects`, `Reminders`, `SettingsNotifications`, `SyncStatus`, `TempIdMapping`, and `User` objects, where renamed to `collaborators`, `collaborator_states`, `day_orders`, `day_orders_timestamp`, `filters`, `items`, `labels`, `live_notifications`, `live_notifications_last_read`, `notes`, `projects`, `reminders`, `settings_notifications`, `sync_status`, `temp_id_mapping`, and `user`, respectively
* The `UserId` variable is not returned by the `sync` call anymore.
* The `timezone` and `tz_offset` properties of the user were replaced by the `tz_info` object.
* The `is_dummy` and `guide_mode` properties of the user were removed.
* The `has_push_reminders`, `beta`, `restrictions`, and `dateist_inline_disabled` properties of the user were moved to the `features` object.
* The `uid` property of labels was removed.
* The `user_id` property of filters was removed.
* The `due_date` property of reminders was removed.
* The `user_id`, `archived_date`, and `archived_timestamp` properties of projects were removed.
* The `due_date` property of items was removed.
* Only 10 notes per item are returned with the `sync` call and thus the `get_item` call should be used for more.
