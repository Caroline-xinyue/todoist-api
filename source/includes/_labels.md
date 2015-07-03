# Labels

> A label in Todoist is a JSON object. Typically a label object will look like:

```shell
{
  "is_deleted": 0,
  "uid": 1,
  "color": 7,
  "id": 1234,
  "name": "Label1"
}
```

```python
{
  'color': 7,
  'is_deleted': 0,
  'uid': 1,
  'name': 'Label1',
  'id': 1234
}
```

### Properties

Property | Description
-------- | -----------
id | The id of the label.
name| The name of the label.
color | The color of the label (a number between `0` and `7`, or between `0` and `12` for premium users).
item_order | Label’s order in the label list.

## Add a label

> An example of adding a label:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "label_add", "temp_id": "f2f182ed-89fa-4bbb-8a42-ec6f7aa47fd0", "uuid": "ba204343-03a4-41ff-b964-95a102d12b35", "args": {"name": "Label1"}}]'
{ ...
  "SyncStatus": {"ba204343-03a4-41ff-b964-95a102d12b35": "ok"},
  "TempIdMapping": {"f2f182ed-89fa-4bbb-8a42-ec6f7aa47fd0": 1234},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.labels.add('Label1')
>>> api.commit()
```

Add a label.

### Required arguments

Argument | Description
-------- | -----------
name | The name of the label.

### Optional arguments

Argument | Description
-------- | -----------
color | The color of the label (a number between `0` and `7`, or between `0` and `12` for premium users).
item_order | Label’s order in the label list.

## Update a label

> An example of updating a label:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "label_update", "uuid": "9c9a6e34-2382-4f43-a217-9ab017a83523", "args": {"id": 1234, "color": 3}}]'
{ ...
  "SyncStatus": {"9c9a6e34-2382-4f43-a217-9ab017a83523": "ok"},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.labels.get_by_id(1234)
>>> label.update(color=3)
>>> api.commit()
```

Update a label.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the label.

### Optional arguments

Argument | Description
-------- | -----------
name | The name of the label.
color | The color of the label (a number between `0` and `7`, or between `0` and `12` for premium users).
item_order | Label’s order in the label list.

## Delete a label

> An example of deleting a label:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "label_delete", "uuid": "aabaa5e0-b91b-439c-aa83-d1b35a5e9fb3", "args": {"id": 1234}}]'
{ ...
  "SyncStatus": {"aabaa5e0-b91b-439c-aa83-d1b35a5e9fb3": "ok"},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.labels.get_by_id(1234)
>>> label.delete()
>>> api.commit()
```

Delete a label.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the label.

## Update multiple orders

> An example of updating the orders of multiple labels at once:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "label_update_orders", "uuid": "1402a911-5b7a-4beb-bb1f-fb9e1ed798fb", "args": {"id_order_mapping": {"1234":  1, "5678": 2}}}]'
{ ...
  "SyncStatus": {
    "517560cc-f165-4ff6-947b-3adda8aef744": {
      "1234": "ok",
      "5678": "ok"
    }
  },
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.labels.update_orders({1234: 1, 5678: 2})
>>> api.commit()
```

Update the orders of multiple labels at once.

### Required arguments

Argument | Description
-------- | -----------
id_order_mapping| A dictionary, where a label id is the key, and the order its value: `label_id: order`.
