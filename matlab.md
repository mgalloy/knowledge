# MATLAB

## Start terminal version

Use the `-nodesktop` switch:

	$ matlab -nodesktop

To go full noninteractive:

	$ matlab -nojvm -nodisplay -nosplash

And to execute a script in batch mode:

	$ matlab -nojvm -nodisplay -nosplash -r "total_sed_cal; exit"

Also see the MATLAB help page,
which lists all options,
[here](http://www.mathworks.com/help/matlab/ref/matlabunix.html).

## Recursively add directories to path

Use `addpath` with `genpath`:

	>> addpath(genpath('/scratch/fexi8823/delft3d_openearth/'))

## Locate source for a routine

Use `which`. How sensible.

	>> which('load')
	built-in (/share/local/matlab-2013a/toolbox/matlab/general/load)

## Read a simple columnated text file

Use `load`:

	>> load('nesting.txt')
	>> nesting
	   252   180
	   252   181
	   252   182
	   252   183
	   252   184

## Stop a program during execution

The `keyboard` command is equivalent to IDL's `stop`,
but it only works with the MATLAB Desktop.

No other good way.

Lame.

## Line continuation

Use the ellipsis:

	DPS = vs_let(Nfs, 'map-sed-series', {1:i_max_map_sed_series}, ...
		'DPS', {1:i_max_DPS 0}, 'quiet!');

## Increment a variable

No increment or compound operators. Use:

	>> i = i + 1

Lame.
