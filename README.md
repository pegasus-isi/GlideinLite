
GlideinLite
===========

This is a sample script which can be used to show how to quickly set up
HTCondor pool overlays on top of other compute resources. A common use
case is having your own submit host, with HTCondor installed and
configured as a central manager, and then submitting jobs to a campus
cluster, with the jobs executing this script. The compute nodes will
be registered into your central manager as HTCondor compute nodes.

There are several software systems which can provide glideins, for
example:

   http://www.uscms.org/SoftwareComputing/Grid/WMS/glideinWMS/

   http://bosco.opensciencegrid.org/

This script is a minimal version of those systems, and you can use the
script if you need something custom. For a larger production system
you should consider GlideinWMS or BOSCO.


Pool Password
-------------

The glideins use pool passwords by default. More information about pool
passwords can be found at:

http://research.cs.wisc.edu/htcondor/manual/v8.2/3_6Security.html#SECTION00463400000000000000

Even with pool passwords, we recommend you use firewall rules on your
central manager to allow only HTCondor network traffic from the
clusters you are running the glideins on.

The central manager needs to be configured for pool passwords. At the
minimum, something like this is neede:

```
SEC_DEFAULT_AUTHENTICATION = REQUIRED

SEC_DAEMON_AUTHENTICATION_METHODS = PASSWORD, FS
SEC_CLIENT_AUTHENTICATION_METHODS = PASSWORD, FS

SEC_ENABLE_MATCH_PASSWORD_AUTHENTICATION = True

SEC_PASSWORD_FILE = /etc/condor/pool_password
```

Usage
-----

```
Usage: run-glidein [-h] [-w PATH] [-t MINS] [-s START] [-p SECRET] -c HOST

  -h        Show this help message
  -c HOST   The hostname of the HTCondor central manager
  -p SECRET The HTCondor pool password.
  -s START  Start expression for the STARTD.
  -t HOURS  Number of hours to accept new jobs. 
  -w PATH   Use PATH as work directory. If not specified, $TMP is used.
  -u URL    URL to a HTCondor tarball to be used.
```

Example
-------

The idea is to submit the *run-glidein* script to your local cluster
scheduler. The jobs should be whole node jobs, with a wall time
matching the glidein and the user workload. For example, if the
workload has jobs that are a maximum 10 hours long, you can submit
a job with a 24 hour walltime, with a glidein with -t 14. The glidein
will accept jobs for 14 hours, and will then allow any jobs to finish
for the maximum of 10 hours.


