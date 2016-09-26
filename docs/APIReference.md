CivetWeb API Reference
=========

CivetWeb is often used as HTTP and HTTPS library inside a larger application. An API is available to integrate the CivetWeb functionality in a larger codebase. This document describes the API. Basic usage examples of the API can be found in [Embedding.md](Embedding.md).

Functions
------

### `mg_check_feature( feature )`

#### Parameters

  - `unsigned feature` - a value indicating the feature to be checked

#### Returns

  - `unsigned` - a value indicating if a feature is available

##### Description

The function `mg_check_feature()` can be called from an application program to check of specific features have been compiled in the civetweb version which the application has been linked to. The feature to check is provided as an unsigned integer parameter. If the function is available in the currently linked library version, a value > 0 is returned. Otherwise the function mg_check_feature() returns the value 0.

The following parameter values can be used:

  - `1` - Serve files (NO_FILES not set during compilation). If this feature is available, the webserver is able to serve files directly from a directory tree.
  - `2` - Support HTTPS (NO_SSL not set during compiletion). If this feature is available, the webserver van use encryption in the client-server connection. SSLv2, SSLv3, TLSv1.0, TLSv1.1 and TLSv1.2 are supported, but which protocols are used effectively is dependent on the options used when the server is started.
  - `4` - support CGI (NO_CGI not set during compilation). If this feature is available, external CGI scripts can be called by the webserver.
  - `8` - support IPv6 (USE_IPV6 set during compilation). The CivetWeb library is capable of communicating over both IPv4 and IPv6, but IPv6 support is only available if it has been enabled at compile time.
  - `16` - support WebSocket (USE_WEBSOCKET set during compilation). WebSockets support is available in the CivetWeb library if the proper options has been used during cimpile time.
  - `32` - support Lua scripts and Lua server pages (USE_LUA set during compilation). CivetWeb supports server side scripting through the Lua language, if that has been enabled at compile time. Lua is an efficient scripting language which is less resource heavy than for example PHP.
  - `64` - support server side JavaScript (USE_DUKTAPE set during compilation). Server side JavaScript can be used for dynamic page generation if the proper options have been set at compile time. Please note that client side JavaScript execution is always available if it has been enabled in the connecting browser.
  - `128` - support caching (NO_CACHING not set during compilation). The webserver will support caching, if it has not been disabled while compiling the library.

Parameter values other than the values mentioned above will give undefined results.


### `mg_start( callbacks, user_data, options )`

#### Parameters

  - `const struct mg_callbacks *callbacks` - a structure with optional callback functions to process requests from the web server.
  - `void *user_data` - a pointer to optional user data
  - `char **options` - a list of options used to initialize the web server

#### Returns

  - `struct mg_context *` - a pointer to a context structure when successful, or NULL in case of failure

##### Description

The function `mg_start()` is the only function needed to call to initialize the webserver. After the function returns and a pointer to a contect structure is provided, it is guaranteed that the server has started and is listening on the designated ports. In case of failure a NULL pointer is returned. The behaviour of the web server is controlled by a list of callback functions and a list of options. The callback functions can do application specific processing of events which are encountered by the webserver. If a specific callback function is not provided, the webserver uses their default callback routines. The options list controls how the webserver should be started and contains settings for for example the ports to listen on, the maximum number of threads created to handle requests in parallel and if settings for SSL encryption.

