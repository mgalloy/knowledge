# File access in Python

Most of these are basic,
but they'll save me from having to look them up
when I need them.


## Reading from a text file

Text is read into a list.
```python
with open("dependencies.txt", "r") as f:
  deps = f.read().split("\n")
```

Alternately:
```python
with open("dependencies.txt", "r") as f:
  deps = f.readlines()
```

or use Numpy's `loadtxt`:
```python
from numpy import loadtxt
deps = loadtxt("dependencies.txt", comments="#", delimiter=",", unpack=False)
```


## Writing to a text file

A list of numbers:
```python
>>> precip_rates = np.linspace(5, 20, num=_n_steps, endpoint=False)
>>> precip_rates
array([  5. ,   6.5,   8. ,   9.5,  11. ,  12.5,  14. ,  15.5,  17. ,  18.5])
```

Write the numbers to a text file,
one number per line:
```python
with open('precip_rates.txt', 'w') as fp:
  for val in precip_rates:
    fp.write(str(val) + '\n')
```

Check the result:
```bash
$ cat precip_rates.txt
5.0
6.5
8.0
9.5
11.0
12.5
14.0
15.5
17.0
18.5
```

Better, use Numpy's `savetxt`:
```python
>>> np.savetxt('precip_rates.txt', precip_rates, fmt='%6.2f')
```

Check the result:
```bash
$ cat precip_rates.txt
  5.00
  6.50
  8.00
  9.50
 11.00
 12.50
 14.00
 15.50
 17.00
 18.50
```


## Reading from a binary file

Use Numpy's `fromfile`:
```python
>>> r = np.fromfile('default_slope.bin', dtype=np.float32, count=-1, sep='')
>>> r.shape
(1276,)
>>> r[0:10]
array([ 0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.], dtype=float32)
```

Swap endianness with:
```python
>>> r.newbyteorder()
```


## Reading a CSV file

Use pandas.
```python
>>> import pandas as pd
```

Read a NEON station file (courtesy JimA) with `read_csv`:
```python
>>> fn = '~/data/NEON/NEON.D04.LAJA.DP1.00004.001.00000.000.035.030.BP_30min.csv'
>>> data = pd.read_csv(fn)
```

Peek at the data with `head`:
```python
>>> data.head()
```

What are the names of the column headers?
```python
>>> data.columns
```

Extract columns by name or by index (with the `values` attribute):
```python
>>> time = data['startDateTime']
>>> pressure = data.values[:,2]
```

**References:**

* [10 Minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html)
[pandas.pydata.org]
* [Cookbook](http://pandas.pydata.org/pandas-docs/stable/cookbook.html)
[pandas.pydata.org]


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

Use the `netCDF4` library:

    >>> import netCDF4

Create an identifier for a netCDF file with temperature values:
```python
>>> T_file = 'atmosphere_bottom_air__temperature.nc'
>>> T_id = netCDF4.Dataset(T_file)
>>> T_id.data_model
'NETCDF4'
>>> T_id.file_format
'NETCDF4'
```

What variables are in the file?
```python
>>> len(T_id.variables)
5
>>> T_id.variables.keys()
[u'mesh', u'y', u'x', u'atmosphere_bottom_air__temperature', u'time']
```

Extract the temperature variable:
```python
>>> T = T_id.variables['atmosphere_bottom_air__temperature']

>>> T.shape
(90, 44, 29)

>>> T[0::]
array([[[ 20.,  20.,  20., ...,  20.,  20.,  20.],
        [ 20.,  20.,  20., ...,  20.,  20.,  20.],
        [ 20.,  20.,  20., ...,  20.,  20.,  20.],
        ...,

>>> T[0::].min()
20.0
>>> T[0::].max()
20.0
```

Close the file:
```python
>>> T_id.close()
```

What about modifying variables?
Open the file in "append" mode ("read" mode is the default):
```python
>>> T_id = netCDF4.Dataset(T_file, 'a')
```

Change the value of a variable, but don't lose the reference:
```python
>>> T_id.variables['atmosphere_bottom_air__temperature'][:] = new_values[:]
```

I can also rename a variable:
```python
>>> T_id.renameVariable('atmosphere_bottom_air__temperature', 'T0')
```

Close the file:
```python
>>> T_id.close()
```


### Alternate: scipy.io.netcdf

For netCDF3 files:

	>>> from scipy.io import netcdf

See examples: http://www-pord.ucsd.edu/~cjiang/python.html


