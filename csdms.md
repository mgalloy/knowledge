# CSDMS

## BMI

BMI = Basic Model Interface.

* [BMI API documentation](http://bmi-forum.readthedocs.org/en/latest/index.html) [rtfd.org]
* Old, but the [BMI SIDL signatures](https://github.com/csdms/bmi/blob/master/bmi.sidl)
* The awkward signature of some BMI functions--using a parameter to
pass information out of the function--is so that memory is not allocated
within the function. Memory must be allocated at the calling level
or above.


## Beach (HPCC)

View the beach cluster report at:
[http://beach.colorado.edu/ganglia/](http://beach.colorado.edu/ganglia/).

Use ***beach-nfs*** for file transfers.

	$ rsync foo mapi8888@beach-nfs:/scratch/mapi8888

The **/scratch** disk is directly connected to ***beach-nfs***
and then NFS mounted to ***beach***.
Can login directly:

    $ ssh mapi8461@beach-nfs

Check my disk quota:

    $ quota -s

I've purchased additional storage for ***beach***.
The drive is automounted as **/nas/data**,
and is available from the head and compute nodes.


## Diluvium (webserver; formaerly River)

The docroot on ***diluvium*** is **/data/web/htdocs/csdms**.

Webserver logs are found in **/usr/local/adm/config/apache/logs**.
In particular, `tail`

* **csdms.colorado.edu-error_log** and
* **error_log**

to see the latest errors.

Restart the CSDMS web server with

	$ sudo /usr/local/httpd/bin/apachectl -k restart

My FTP directory on ***river*** is **/data/ftp/pub/users/mapi8888**.
Users can access this site [here](http://csdms.colorado.edu/pub/users/mapi8888/).

CSDMS has a THREDDS server at http://csdms.colorado.edu/thredds/catalog.html.
Data can be added to existing directories at **/data/thredds/public**.
If a new directory is made,
it needs to be entered in the TDS configuration file,
**/usr/local/adm/config/tomcat/content/thredds/catalog.xml**,
and the Apache Tomcat server needs to be restarted with

    $ sudo /etc/init.d/tomcat stop
	$ sudo /etc/init.d/tomcat start


## WMT client and server location

The WMT client is located at **river:/data/web/htdocs/csdms**.
The URL is [https://csdms.colorado.edu/wmt](https://csdms.colorado.edu/wmt).
I also have a testing site at
[https://csdms.colorado.edu/wmt-testing](https://csdms.colorado.edu/wmt-testing).

Use the `wmt_load` script (located in **~/bin**)
to transfer the WMT **war** contents
to the web server docroot.

The WMT _database_ and _data_ servers are located
at **river:/data/web/htdocs/wmt**
(not in the docroot!).

The WMT _execution_ server is located
at **beach:/home/csdms/wmt**.
(And my customized Dakota execution server is
at **beach:/home/csdms/wmt/dakota-tutorial**.)

Output from WMT can be picked up on the CSDMS FTP site.
The files are located at **river:/data/ftp/pub/users/wmt**.


## Build and deploy the WMT client

1. Change to the project directory on, e.g., ***solaria***:

        $ cd ~/projects/mdpiper/wmt-client

1. Call `ant` to build WMT:

        $ ant build
        $ /usr/local/opt/ant@1.9/bin/ant build  # needed on solaria; 2017-07-24

1. Sync the resulting **war/** directory (war = "web archive") with
   that in my home directory on ***river***:

        $ rsync -r war mapi8461@river:~

   Note carefully the _src_ and _dst_ directories.

   Or, duh, use the script I wrote to do just this:

        $ wmt_rsync

1. Login to ***river***.

1. Optionally edit the **war/config.json** file.

1. From `$HOME`, call the `wmt_load` script, optionally with the `-t`
   flag to copy to the `wmt-testing` URL.

        $ wmt_load -t


## Add a component to WMT

1. Add component metadata to the
   [https://github.com/csdms/component_metadata](https://github.com/csdms/component_metadata)
   project.
1. Pull the changes into the WMT server:

		$ cd /data/web/htdocs/wmt/api/dev/db/components
		$ git pull

1. Restart the CSDMS web server.

1. Refresh WMT in your browser and login.

### Notes

* The `id` field of the **info.json** file must match the name of the
  directory holding the component; e.g., **vector\_parameter\_study**.


## Create a WMT database server

1. On ***river*** (for example), copy the files from an existing database server:

		$ cd /data/web/htdocs/wmt/api
		$ cp -R dev dakota

	The path to the new server is **/data/web/htdocs/wmt/api/dakota**. Look at
	the directory structure:

        $ l
		bin/  conf/  db/  files/  internal/  logs/  static/  templates/

1. Change the permissions on **internal** so I can access it without sudo:

        $ sudo chown mapi8461:nobody internal/

1. Get the latest miniconda distribution and install it in **internal/miniconda**.

   Afterward, it may be necessary to activate the root environment:

        $ source ./miniconda/bin/activate root

1. Install the packages 'paramiko' and 'passlib' with `conda`, and
   'wsgilog' and 'requests-toolbelt' with `pip`:

        $ ./miniconda/bin/conda install paramiko passlib
		$ ./miniconda/bin/pip install wsgilog requests-toolbelt

1. Add location of server to **/usr/local/adm/config/apache/conf/httpd.conf**. E.g.:

        #WSGIDaemonProcess wmt-api-dakota threads=15 maximum-requests=10000 python-path=/data/web/htdocs/wmt/api/dakota/internal:/data/web/htdocs/wmt/api/dakota/internal/miniconda/lib/python2.7/site-packages python-home=/data/web/htdocs/wmt/api/dakota/internal/miniconda
		WSGIDaemonProcess wmt-api-dakota threads=15 maximum-requests=10000 python-path=/data/web/htdocs/wmt/api/dakota/internal
		WSGIProcessGroup wmt-api-dakota
		WSGIScriptAlias /wmt/api-dakota /data/web/htdocs/wmt/api/dakota/bin/start_wmt.wsgi/ process-group=wmt-api-dakota

		Alias /wmt/api-dakota/static /data/web/htdocs/wmt/api/dakota/static
		<Directory /data/web/htdocs/wmt/api/dakota/>
		  Order deny,allow
		  Allow from all
		</Directory>

1. Edit the server URL information in **conf/wmt.ini**, using the script
   alias from the **httpd.conf** file. E.g.:

    ```
    [url]
    scheme = https
    netloc = csdms.colorado.edu
    path = wmt/api-topoflow
    ```

    HTTPS required.

1. Restart the web server.


### Modifications

* Edit **/data/web/htdocs/wmt/api/dakota/internal/wmt/models/model.py**, Line 153: comment out. (No `exchange_items`)
* Same file, Line 172: comment out. (No `run_duration`)


## Create a WMT execution server

This installs the CSDMS software stack on a host machine.

1. On ***beach*** (for example), add a new directory to **/home/csdms/wmt**.

		$ cd /home/csdms/wmt
		$ mkdir dakota-tutorial && cd dakota-tutorial

1. Check that Ruby (located in **/usr/local/ruby/bin**) is in the
   path.

	* I also removed my default Anaconda Python from the path; it was
	  installing packages in **~/.local** instead of the
	  **site-packages** directory of the minipython distro included
	  with the CSDMS stack.

1. Get the install script:

		$ wget https://raw.githubusercontent.com/csdms/wmt-exe/master/scripts/install

1. Check the Java version installed on the host using a recent build
   of the execution server:

		$ cd /home/csdms/wmt/20-04-2015
		$ ./local/bin/babel-config --dump | grep -i java
		$ cd -

1. Run the install script:

		$ python install --with-java=/usr/java/default/bin/java $PWD

	Note that this is the system Python (2.6.6); I removed Anaconda from my path. 

It takes about an hour to build.

### Modifications 2015-04-29

I couldn't build `bmi-babel` and `wmt-exe`:
`setuptools` (I'm pretty sure) complained about
the **site-packages** directory not being in `PYTHONPATH`.
As a work-around,
I added these directories to `PYTHONPATH` and installed manually
(i.e., from the shell prompt, not the script).
Here's how I installed `wmt-exe`:

	$ /home/csdms/wmt/dakota-tutorial/opt/conda/envs/wmt/bin/python setup.py develop \
		--install-dir=/home/csdms/wmt/dakota-tutorial/opt/conda/envs/wmt/lib/python2.7/site-packages \
		--script-dir=/home/csdms/wmt/dakota-tutorial/bin

I also installed `coupling` and my Dakota package (`dakota`) with the same syntax,
although I used `--script-dir=/home/csdms/wmt/dakota-tutorial/local/bin`.

### Modifications 2015-07-06

When building an execution server for TopoFlow,
`brew` failed to find a formula for `zlib`
(required for `child`),
even though Eric included it in `homebrew/dupes/zlib`.
Install it with:

    $ ./local/bin/brew install homebrew/dupes/zlib

After installing `zlib` and rerunning the install script,
`pkg-config` was not found.
Install it with:

    $ ./local/bin/brew install pkg-config

Homebrew complained when installing `esmf` that `netcdf` was found in
more than one tap.
I edited the ESMF formula in **./local/Library/Taps/csdms/homebrew-tools**
to use `csdms/dupes/netcdf`.


## Build Babel-ized components for execution server

The [bmi-forum/bmi-babel](https://github.com/bmi-forum/bmi-babel)
package must be installed on the execution server.

Get the desired BMI-ed sources from GitHub:

    $ bmi-babel-fetch https://github.com/mcflugen/sedflux,add-function-pointers https://github.com/mdpiper/storm --prefix=$(pwd)/local > bmis.yaml

This creates the directory **cache**
to store the downloaded repos from GitHub.
All the BMI descriptions are included in **bmis.yaml**;
a single file is required.
The **local** install target directory might be revised.

Next, make the Bocca project that wraps BMI implementations as Babel classes:

    $ bmi-babel-make bmis.yaml

This makes the directory **csdms**. 

Make and install the components:

    $ cd csdms/
	$ ./configure
	$ make all install

The directory **./local/lib** must be in `LD_LIBRARY_PATH`.

To make the CMI components callable,
in the `coupling` package,
add the `cmt.framework.bmi_bridge` module,
then add and edit the `cmt.components` module.
(Eric has these in **/home/csdms/wmt/bmi-tutorial**,
but they're not yet included in the `coupling` package.)


### Debugging

When using components in Python, unhelpful errors such as

    _Exception

may be generated by Babel.
Use `bocca` to view/repair implementations:

    $ cd csdms/
    $ bocca edit GC2D -i

Note that I can edit the Python code for debugging.
Eric recommends wrapping a `try` statement
around the return to view the error message.
(We may add this to all functions in the Babel wrappers.)
Here's an example using `initialize`,
including printing the error to a file
that appears in the run directory:

    # DO-NOT-DELETE splicer.begin(initialize)
    try:
	    self._model.initialize(config_file)
	except Exception as err:
	    print >> sys.stderr, str(err)
		with open('err.txt', 'w') as fp:
		    fp.write(str(err))
		raise ValueError(str(err))

    return 0
	# DO-NOT-DELETE splicer.end(initialize)


## Point data server to execution server

Edit **/data/web/htdocs/wmt/api/dakota/db/hosts/beach.colorado.edu/db/info.json**
to point to the new execution server.

	{
	  "name": "beach.colorado.edu",
	  "wmt_prefix": "/home/csdms/wmt/dakota-tutorial"
	}

The execution server scripts are in **/home/csdms/wmt/bin** on ***beach***.
I copied them to **/home/csdms/wmt/dakota-tutorial/bin**
and edited them to use the new execution server path.

### Modifications

* Edited **wmt_slave.py**, line 380: "file" keyword should be "filename".
* Added Dakota paths to **wmt-env.sh**. Dakota is already installed on
  ***beach*** in **/usr/local/dakota**.
* I added **etc/wmt/environ.yaml** and modified it to use the paths in
  **wmt-env.sh**.


## Run simulation from the command line

To run a simulation from the command line on the executor,
use:
```
$ wmt-slave <uuid> --server-url=https://csdms.colorado.edu/wmt/api-testing
```
The output from the run will be in **~/.wmt/<uuid>**.

This is particularly helpful for debugging.
Console output (standard output and standard error)
from the run is logged in **~/.wmt/<uuid>/stdout**.


## Installing JupyterHub on ***siwenna***

Following installation instructions at
https://github.com/jupyterhub/jupyterhub.

Use Python 3 in **/usr/local/anaconda3**.

    PATH=/usr/local/anaconda3/bin:$PATH

(I added this to the **.bash_profile** of root.)

Install `npm` and `nodejs`.

    sudo yum install npm nodejs

This also installs the `node` executable.

Install HTTP proxy and dependencies.

    sudo npm install -g inherits@'2'
	sudo npm install -g configurable-http-proxy

Install JupyterHub.

    sudo /usr/local/anaconda3/bin/pip install jupyterhub

This also retrieves and installs some dependencies.

Install Jupyter Notebook.

    sudo /usr/local/anaconda3/bin/pip install --upgrade notebook

This installed a slew of dependencies.


## Configuring and running JupyterHub on ***siwenna***

The
[Getting Started](https://jupyterhub.readthedocs.io/en/0.7.2/getting-started.html)
document for version 0.7.2 (installed on ***siwenna***)
was very helpful.

As a test, run JupyterHub as a (nonpriviledged) single-user server.

    jupyterhub

Now, from a browser running locally on ***siwenna***,
I can log in with my PAM credentials at http://127.0.0.1:8000
and get a Jupyter Notebook.

To allow others to log in,
run JupyterHub as root.
Executing

    $ su -
    # jupyterhub --ip 128.138.87.12 --debug

allows me to log in
at http://128.138.87.12:8000.
Jupyter Notebook is started in my home directory.
(This works even when the firewall is enabled.)
This command also allows me to log in
at http://siwenna.colorado.edu:8000.
Yay!

I want to support HTTPS.
Executing

	# jupyterhub --ip 128.138.87.12 --ssl-key /etc/pki/tls/private/ca.key --ssl-cert /etc/pki/tls/certs/ca.crt --debug

allows me to log in
at https://siwenna.colorado.edu:8000,
although the user has to accept my hinky, self-generated, certificate,
and they can no longer access the site via HTTP.
Yay!

Use a JupyterHub configuration file
instead of command line args.
Create a config file with

    jupyterhub --generate-config

Install a cookie secret file in **/srv/jupyterhub** with

    openssl rand -base64 2048 > /srv/jupyterhub/cookie_secret

and add this to the `cookie_secret_file` attribute
of the config file.
Be sure to set permissions of 600 (u=rw) on this file.

Generate a 32-character hex string with `openssl`
to use as the proxy authentication token.
Set it as the `proxy_auth_token` attribute of the config file.

Place **jupyterhub_config.py** in **/etc/jupyterhub**
and call JupyterHub with

    # jupyterhub -f /etc/jupyterhub/jupyterhub_config.py


### Think about

* Can I start JupyterHub as a service when ***siwenna*** starts?
See https://github.com/jupyterhub/jupyterhub/wiki/Run-jupyterhub-as-a-system-service.
* It would be great to use the
[OAuthenticator](https://github.com/jupyterhub/oauthenticator)
so that people could log in with, e.g.,
their GitHub or Google (or CSDMS!) credentials.
