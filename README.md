fiapy
=====

## What's it ?

[IEEE1888](http://standards.ieee.org/findstds/standard/1888-2014.html) is a set of protocol to exchange amount of time series data between two nodes.

This is an IEEE1888 implementation written in python.
IEEE1888.3 security is almost implemented.
checking the peer's subjectAltName has not been done yet.
WRITE and FETCH protocols for both client and server are supported.
TRAP protocol is under developing.
REGISTER and LOOKUP are not supported.
Experimentaly, JSON format and translation between XML and JSON are implemented.

### fiapy.py

It's an IEEE1888 server implementation.
To run it, you can simply type like below:

    ~~~~
    % fiapy.py
    ~~~~

The default port number is 18880.
If you want to see the debug message, you add "-d 99" as the parameter.

If you want to use the IEEE1888.3 security, you simply add "-s" option.

    ~~~~
    % fiapy.py -s
    ~~~~

The default port number is 18883.

It's not allowed to use both secure and non-secure mode in same time.

### fiapClient.py

It's an IEEE1888 client implementation.
It reads a request file from standard input or a file with -c option.
By default, it send a request in JSON format and prints the result in JSON format.
If you provide a file in XML format, it automatically translates into JSON.

For example, to send a FETCH request in JSON,

~~~~
% fiapClient.py -e http://fiap.example.org/storage -c test-fetch-01.json
~~~~

If you want to send a request in XML, you have to add "-x" option into the parameter.
If you want to see the result in XML, you have to add "-X" option.

### trapy.py

TBD

## System requirement

python version is 2.7.
you need addtional python modules.
you have to use the mongodb if you use the IEEE1888 server.

### python modules

the following modules are required.  almost modules are installed with the core module.
dateutil, pytz and pymongo are probably needed to be intalled.

    ~~~~
    Base64
    BaseHTTPServer
    SocketServer
    argparse
    bson
    dateutil
    hmac
    httplib2
    json
    mimetools
    pymongo
    pytz
    rfc822
    urlparse
    uuid
    ~~~~

    **TO BE CHECKED**: it probably includes the modules for urllib.

### mongodb

this version requires mongodb as the backend database.
you need to configure properly and prepare the port number for the mongodb server.
you have to specify the port number when you launch fiapy.py.

## TODO

- implement TRAP protocol, trapy.
- implement GET method.
- consider the error message when the translation error happens.
- consider the error message when the method hasn't been identified.

## IEEE1888.3 security

See HOWTO-cert.md to create certificates for the IEEE1888 component.

## Timezone

The timezone of data in the database is UTC.
If an offset of the datetime string in the point element is specified,
fiapy converts it into UTC before saving the data.
If any offset is not specified and the timezone is defined in the JSON data,
fiapy uses the timezone in the JSON data to convert it into UTC.
If both an offset of the datetime string and the timezone in the JSON data,
fiapy uses the offset of the string.
If any timezone information are not specified,
fiapy deals with it as UTC timezone.

## JSON format

see JSON-data-model.md

## Tips

### How to send a IEEE1888 POST request simply.

if you want to send a request in XML without fiapClient.py,
you can use test-post-xml.sh to send it.
It just utilizes "wget --post-file".

