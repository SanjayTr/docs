# httpserver.repy

This library abstracts away the details of the HTTP protocol, providing an alternative to calling a user-supplied function on each request. The return value of the user-supplied function determines the response that is sent to the HTTP client. This allows the end user to not have to worry about the semantics of communicating with an HTTP server.

In many ways, this module is analogous to the http.server module in Python documentation. See http://docs.python.org/py3k/library/http.server.html.


### Classes & Functions

```
def httpserver_registercallback(addresstuple, cbfunc):
```
   Registers a callback function on the (host, port).

   Notes: 

   * addresstuple is an address 2-tuple to bind to: ('host', port).
   * cbfunc is the callback function to process requests. It takes one argument, which is a dictionary describing the HTTP request. It looks like      this (just an example):
        {
          'verb': 'HEAD',
          'path': '/',
          'querystr': 'foo=bar&baz',
          'querydict': { 'foo': 'bar', 'baz': None }
          'version': '0.9',
          'datastream': object with a file-like read() method,
          'headers': { 'Content-Type': 'application/x-xmlrpc-data'},
          'httpdid': 17,
          'remoteipstr': '10.0.0.4',
          'remoteportnum': 54001
        }
      ('datastream' is a stream of any HTTP message body data sent by the
      client.)

      It is expected that this callback function returns a dictionary of:
        {
          'version': '0.9' or '1.0' or '1.1',
          'statuscode': any integer from 100 to 599,
          'statusmsg' (optional): an arbitrary string without newlines,
          'headers': { 'X-Header-Foo': 'Bar' },
          'message': arbitrary string
        }
   * Exceptions (TypeError, ValueError, KeyError, IndexError) are raised if arguments to this function are malformed.
   * The (hostname, port) tuple cannot be restricted, already taken, etc.
   * Returns a handle for the listener (an httpdid). This can be used to stop the server.


```
def httpserver_stopcallback(callbackid):
```
   Removes an existing callback function, i.e. stopping the server.

   Notes: 

   * callbackid is the id returned by httpserver_registercallback().
   * Raises IndexError or KeyError if the id is invalid or has already been deleted.


### Includes

[wiki:SeattleLib/urllib.repy]

[wiki:SeattleLib/urlparse.repy]

[wiki:SeattleLib/uniqueid.repy]

[wiki:SeattleLib/sockettimeout.repy]

[wiki:SeattleLib/httpretrieve.repy]

