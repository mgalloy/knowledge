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

Examine the result:
```bash
$ cat contact.yaml
city: Boulder
name: Mark Piper
state: Colorado
street1: CSDMS
street2: 4001 Discovery Dr.
zip: 80303
```

## NetCDF

For netCDF3 files:

	>>> from scipy.io import netcdf

See examples: http://www-pord.ucsd.edu/~cjiang/python.html


