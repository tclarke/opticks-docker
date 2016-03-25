Opticks Docker Image
====================
Builds a docker image containing [Opticks](http://opticks.org) along with the spectral extension.

Building with the default paramters will install the latest production release of Opticks. Build a different version
by specifying `--build-arg opticks_version=4.12.0` etc. The following build args are known:
* `opticks_version` the version string for Opticks
* `opticks_rpm` the RPM name, includes the RPM revision and any other specifics (such as noldap)
* `opticks_url` the url to download the RPM.
* `spectral_version` the version string for the spectral extension
* `spectral_url` the url or path to download the AEB. If you have a local copy, specify it by overriding this value.

Running opticks batch
=====================
A `/wizards` volume is defined. You'll typically want to volume mount your wizard files here. You may also want to volume
mount some data.

The default behavior is to execute `/wizards/wizard.batchwiz` in OpticksBatch.
Specify a different wizard to the run command with `-input:/wizards/mywizard.batchwiz` or specify other OpticksBatch arguments.

Running the opticks GUI
=======================
You can also run the Opticks GUI if your host has an X11 server.

Start opticks with `docker run -d -e DISPLAY=my-host-ip:0 --entrypoint /opt/Opticks/Bin/Opticks opticks`

You'll need to allow connections from the docker image. This can be done with `xhost +` but this is very insecure so it's preferable to use xauth or at least limit connections to the docker IP.
A good compromise is to do:
```
my_ip=10.0.0.1 # set your local machine's IP address here
xhost +${my_ip}
docker run -v $(pwd):/wizards -d -e DISPLAY=${my_ip}:0 --net=host --entrypoint /opt/Opticks/Bin/Opticks opticks
```
