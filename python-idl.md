# Mapping IDL to Python

Equivalent (or nearly so) commands in Python and IDL.

Python         | IDL
:-----         | :--
`dir`          | HELP
`help`         | ONLINE_HELP
`pickle`       | SAVE
`range()`      | INDGEN
`np.arange()`  | INDGEN (Numpy)
`linspace()`   | INDGEN(n)*scale + offset
`f.seek()`     | SKIP_LUN, POINT_LUN
`f.tell()`     | POINT_LUN
`len()`        | N_ELEMENTS (only on first dim of array)
`np.size()`    | N_ELEMENTS
`np.shape()`   | SIZE
`np.reshape()` | REFORM
`None`         | !NULL
`os.chdir()`   | CD
`os.getcwd()`  | CD
`>`            | GE
`<`            | LE
`eval()`       | EXECUTE
`exec()`       | EXECUTE
`sys.exit()`   | EXIT
`a = x.copy()` | a = x (Numpy)
`plt.plot()`   | PLOT()
`plt.figure()` | WINDOW()
