# File access in Python

## Reading from a text file

Text is read into a list.

```python
with open("dependencies.txt", "r") as f:
  deps = f.read().split("\n")
```

## JSON and YAML


## NetCDF

For netCDF3 files:

	>>> from scipy.io import netcdf

See examples: http://www-pord.ucsd.edu/~cjiang/python.html


