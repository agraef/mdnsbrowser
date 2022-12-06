# mdnsbrowser

This is a Pd external which lets you publish and discover network services via mDNS/DNS-SD a.k.a. Zeroconf. It works with the prevalent Zeroconf implementations on Linux and MacOS, [Avahi](https://www.avahi.org/) and [Bonjour](https://developer.apple.com/bonjour/). One obvious application are services which transmit [OSC](http://opensoundcontrol.org/) (Open Sound Control) between mobile applications such as [TouchOSC](https://hexler.net/touchosc/) and Pd. But this external may conceivably be used to advertise any kind of networked-based service between Pd and other host-based or mobile applications.

NOTE: This is based on an earlier external which is still available from [Bitbucket](https://bitbucket.org/agraef/pd-mdnsbrowser). The present module is now Lua-based and thus much easier to install than the previous module written in the Pure language.

## License

mdnsbrowser is Copyright (c) 2022 by Albert Gräf. It is distributed under the 3-clause BSD license, please check the COPYING file for details.

## Requirements and Installation

The external is written in Lua, so pd-lua is required. Lua 5.4 has been tested, Lua 5.3 likely works as well, but earlier Lua versions probably require some fiddling with the sources. Lua can be found in many Linux distributions and on [Homebrew](https://brew.sh). A version of pd-lua compatible with Lua 5.3 and 5.4 is available from <https://github.com/agraef/pd-lua>.

You'll also need Avahi on Linux (readily available in most Linux distributions) and Bonjour on MacOS (comes preinstalled on Mac computers). On Linux you'll also have to make sure that the avahi-daemon service is running (`systemctl start avahi-daemon`; see, e.g., <https://wiki.archlinux.org/title/avahi> for details).

Run `make` to compile the Lua mdns module. The Makefile should figure out on its own whether it runs on Linux or Mac and compile the right source module (avahi.c on Linux, bonjour.c on Mac) to mdns.so.

After running `make` you can immediately test that the external works by opening the mdnsbrowser-help.pd patch in Pd. If you're getting a bunch of error messages from pd-lua complaining that it couldn't find the mdns module, then most likely you've forgotten to run `make`; this module needs to be present so that mdnsbrowser.pd_lua will work. On the other hand, if Pd just complains that it couldn't create the mdnsbrowser object then pd-lua is missing; make sure that pd-lua is installed and in Pd's startup libraries.

Also note that the included help patch assumes that you have the [mrpeach](https://github.com/pd-externals/mrpeach) externals installed and on Pd's search path. If you're getting a bunch of creation errors for externals like udpsend and udpreceive when loading the help patch, it's a sure sign that mrpeach is missing. Try to install it from Deken or compile it from source; it's not that hard to do.

Installation isn't necessary, the external can simply be run in-place, editing the provided help patch to adjust it for your needs. But if you want to make sure that the mdnsbrowser object and the help patch can be invoked from anywhere, you can just copy the entire mdnsbrowser directory to some convenient location and make sure that you add that directory to Pd's search path. The Pd FAQ has an [article][1] about some common locations on the various platforms that you might want to use.

[1]: https://puredata.info/docs/faq/how-do-i-install-externals-and-help-files

## Usage

Please check the included help patch for usage information and the comments at the beginning of the mdnsbrowser.pd_lua script for further information. In brief, the external is invoked as:

    mdnsbrowser [name [type] [port]]

The arguments are all optional, but if any are specified, you need to give at least the service name, possibly followed by a service type and a port number. Reasonable defaults (service name `OSC`, type `_osc._udp`, and port number 8000) will be provided which match TouchOSC's defaults. mdnsbrowser will also tack on your system's hostname to the service name to make it easier to identify different instances of mdnsbrowser running on different computers on your local network.

Note that mdsnbrowser only helps you advertise OSC endpoints and discover them in your local network, it does *not* implement the actual transport of OSC messages from/to your mobile devices. You'll still have to use either Pd's built-in facilities or the mrpeach externals to do that. The included help patch shows how this can be implemented using the mrpeach externals.

## Feedback and Bug Reports

As usual, bug reports, patches, feature requests, other comments and source contributions are more than welcome. Just drop me an email, file an issue or send me a pull request on mdnsbrowser's GitHub page.

Enjoy! :)

Albert Gräf <aggraef@gmail.com>
