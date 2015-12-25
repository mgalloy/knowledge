# File access in Python

## Reading from a text file

Text is read into a list.

```python
with open("dependencies.txt", "r") as f:
  deps = f.read().split("\n")
```

## JSON and YAML

JSON and YAML files can easily be created from a dict.

```python
>>> contact = {'name':'Mark Piper', 'street1':'CSDMS', 'street2':'4001 Discovery Dr.', 'city':'Boulder', 'state':'Colorado', 'zip':80303}
```

Dump the dict to a YAML file:
```python
import yaml
with open('contact.yaml', 'w') as fp:
  yaml.dump(contact, fp, default_flow_style=False)
```

and examine the result:
```bash
$ cat contact.yaml
city: Boulder
name: Mark Piper
state: Colorado
street1: CSDMS
street2: 4001 Discovery Dr.
zip: 80303
```

Dump the dict to a JSON file:
```python
import json
with open('contact.json', 'w') as fp:
  json.dump(contact, fp, indent=2)
```

and examine the result:
```bash
$ cat contact.json
{
  "city": "Boulder",
  "name": "Mark Piper",
  "zip": 80303,
  "street1": "CSDMS",
  "street2": "4001 Discovery Dr.",
  "state": "Colorado"
}
```

Load the resulting JSON and YAML files back into Python:
```python
with open('contact.json', 'r') as fp:
  json_contact = json.load(fp)

with open('contact.yaml', 'r') as fp:
  yaml_contact = yaml.load(fp)
```

Examine the results:
```python
>>> json_contact
{u'city': u'Boulder',
 u'name': u'Mark Piper',
 u'state': u'Colorado',
 u'street1': u'CSDMS',
 u'street2': u'4001 Discovery Dr.',
 u'zip': 80303}

>>> yaml_contact
{'city': 'Boulder',
 'name': 'Mark Piper',
 'state': 'Colorado',
 'street1': 'CSDMS',
 'street2': '4001 Discovery Dr.',
 'zip': 80303}
```


## NetCDF

For netCDF3 files:

	>>> from scipy.io import netcdf

See examples: http://www-pord.ucsd.edu/~cjiang/python.html

Better to use the `netCDF4` library:

    >>> import netCDF4

