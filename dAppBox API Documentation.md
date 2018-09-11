# API
An application program interface (API) is code that allows two software programs to communicate with each other. The API defines the correct way for a developer to write a program that requests services from an operating system (OS) or other application.

APIs are made up of two related elements. The first is a specification that describes how information is exchanged between programs, done in the form of a request for processing and a return of the necessary data. The second is a software interface written to that specification and published in some way for use.
# RESTful API or REST API
A RESTful API is an application program interface (API) that uses HTTP requests to GET, PUT, POST and DELETE the data.
A RESTful API -- also referred to as a RESTful web service -- is based on representational state transfer (REST) technology, an architectural style and approach to communications often used in web services development.

REST technology is generally preferred to the more robust Simple Object Access Protocol (SOAP) technology because REST leverages less bandwidth, making it more suitable for internet usage.

A RESTful API breaks down a transaction to create a series of small modules. Each module addresses a particular underlying part of the transaction. This modularity provides developers with a lot of flexibility, but it can be challenging for developers to design from scratch.

REST is an acronym for Representational State Transfer. It is a web standards architecture and HTTP Protocol. The REST protocol, describes six (6) constraints:

1. Uniform Interface
2. Cacheable
3. Client-Server
4. Stateless
5. Code on Demand
6. Layered System

REST is composed of methods such as a base URL, media types, etc. RESTful applications uses HTTP requests to perform the CRUD (Create, Read, Update, and Delete) operations.

In dAppBox the REST API is stored in gui.go file of the dAppBoxService. 
    
     s.createMetaJSFile()

    // The GET handlers
    getRestMux := http.NewServeMux()
    getRestMux.HandleFunc("/rest/db/completion", s.getDBCompletion) // device folder
    getRestMux.HandleFunc("/rest/db/file", s.getDBFile) // folder file
    getRestMux.HandleFunc("/rest/db/ignores", s.getDBIgnores) // folder
    getRestMux.HandleFunc("/rest/db/need", s.getDBNeed) // folder [perpage] [page]
    getRestMux.HandleFunc("/rest/db/remoteneed", s.getDBRemoteNeed) // device folder [perpage] [page]
    getRestMux.HandleFunc("/rest/db/status", s.getDBStatus) // folder
    getRestMux.HandleFunc("/rest/db/browse", s.getDBBrowse) // folder [prefix] [dirsonly] [levels]
    getRestMux.HandleFunc("/rest/folder/versions", s.getFolderVersions) // folder
    getRestMux.HandleFunc("/rest/folder/pullerrors", s.getPullErrors) // folder
    getRestMux.HandleFunc("/rest/events", s.getIndexEvents) // [since] [limit] [timeout] [events]
    getRestMux.HandleFunc("/rest/events/disk", s.getDiskEvents) // [since] [limit] [timeout]
    getRestMux.HandleFunc("/rest/stats/device", s.getDeviceStats) // -
    getRestMux.HandleFunc("/rest/stats/folder", s.getFolderStats) // -
    getRestMux.HandleFunc("/rest/svc/deviceid", s.getDeviceID) // id
    getRestMux.HandleFunc("/rest/svc/lang", s.getLang) // -
    getRestMux.HandleFunc("/rest/svc/report", s.getReport) // -
    getRestMux.HandleFunc("/rest/svc/random/string", s.getRandomString) // [length]
    getRestMux.HandleFunc("/rest/system/browse", s.getSystemBrowse) // current
    getRestMux.HandleFunc("/rest/system/config", s.getSystemConfig) // -
    getRestMux.HandleFunc("/rest/system/config/insync", s.getSystemConfigInsync) // -
    getRestMux.HandleFunc("/rest/system/connections", s.getSystemConnections) // -
    getRestMux.HandleFunc("/rest/system/discovery", s.getSystemDiscovery) // -
    getRestMux.HandleFunc("/rest/system/error", s.getSystemError) // -
    getRestMux.HandleFunc("/rest/system/ping", s.restPing) // -
    getRestMux.HandleFunc("/rest/system/status", s.getSystemStatus) // -
    getRestMux.HandleFunc("/rest/system/upgrade", s.getSystemUpgrade) // -
    getRestMux.HandleFunc("/rest/system/version", s.getSystemVersion) // -
    getRestMux.HandleFunc("/rest/system/debug", s.getSystemDebug) // -
    getRestMux.HandleFunc("/rest/system/log", s.getSystemLog) // [since]
    getRestMux.HandleFunc("/rest/system/log.txt", s.getSystemLogTxt) // [since]

    getRestMux.HandleFunc("/rest/system/folderdata", s.getFolderData) // -
    getRestMux.HandleFunc("/rest/system/graphdata", s.getGraphData) // -
    getRestMux.HandleFunc("/rest/system/qrdata", s.getQrRaw) // -
    getRestMux.HandleFunc("/rest/system/ethereuminfo", s.getEthereumInfo) // -

    // The POST handlers
    postRestMux := http.NewServeMux()
    postRestMux.HandleFunc("/rest/db/prio", s.postDBPrio) // folder file [perpage] [page]
    postRestMux.HandleFunc("/rest/db/ignores", s.postDBIgnores) // folder
    postRestMux.HandleFunc("/rest/db/override", s.postDBOverride) // folder
    postRestMux.HandleFunc("/rest/db/scan", s.postDBScan) // folder [sub...] [delay]
    postRestMux.HandleFunc("/rest/folder/versions", s.postFolderVersionsRestore) // folder <body>
    postRestMux.HandleFunc("/rest/system/config", s.postSystemConfig) // <body>
    postRestMux.HandleFunc("/rest/system/error", s.postSystemError) // <body>
    postRestMux.HandleFunc("/rest/system/error/clear", s.postSystemErrorClear) // -
    postRestMux.HandleFunc("/rest/system/ping", s.restPing) // -
    postRestMux.HandleFunc("/rest/system/reset", s.postSystemReset) // [folder]
    postRestMux.HandleFunc("/rest/system/restart", s.postSystemRestart) // -
    postRestMux.HandleFunc("/rest/system/shutdown", s.postSystemShutdown) // -
    postRestMux.HandleFunc("/rest/system/upgrade", s.postSystemUpgrade) // -
    postRestMux.HandleFunc("/rest/system/pause", s.makeDevicePauseHandler(true)) // [device]
    postRestMux.HandleFunc("/rest/system/resume", s.makeDevicePauseHandler(false)) // [device]
    postRestMux.HandleFunc("/rest/system/debug", s.postSystemDebug) // [enable] [disable]

    // Debug endpoints, not for general use
    debugMux := http.NewServeMux()
    debugMux.HandleFunc("/rest/debug/peerCompletion", s.getPeerCompletion)
    debugMux.HandleFunc("/rest/debug/httpmetrics", s.getSystemHTTPMetrics)
    debugMux.HandleFunc("/rest/debug/cpuprof", s.getCPUProf) // duration
    debugMux.HandleFunc("/rest/debug/heapprof", s.getHeapProf)
    getRestMux.Handle("/rest/debug/", s.whenDebugging(debugMux))

    // A handler that splits requests between the two above and disables
    // caching
    restMux := noCacheMiddleware(metricsMiddleware(getPostHandler(getRestMux, postRestMux)))

    // The main routing handler
    mux := http.NewServeMux()
    mux.Handle("/rest/", restMux)
    mux.HandleFunc("/rest/qr/", s.getQR)

    }

The code above creates a controller for each endpoint, then expose an HTTP server on port.

Note: We are using GET, POST, and DEBUG where appropriate. We are also defining parameters that can be passed in.

Some handler functions written as an argument inside another function named AuthenticationMiddleware. This is part of implementing Authentication which is discussed a bit later.

     func getPostHandler(get, post http.Handler) http.Handler {
         return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
             switch r.Method {
             case "GET":
                 get.ServeHTTP(w, r)
             case "POST":
                 post.ServeHTTP(w, r)
             default:
                 http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
             }
         })
     }

     func debugMiddleware(h http.Handler) http.Handler {
         return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
             t0 := time.Now()
             h.ServeHTTP(w, r)

             if shouldDebugHTTP() {
                 ms := 1000 * time.Since(t0).Seconds()

                 // The variable `w` is most likely a *http.response, which we can't do
                 // much with since it's a non exported type. We can however peek into
                 // it with reflection to get at the status code and number of bytes
                 // written.
                 var status, written int64
                 if rw := reflect.Indirect(reflect.ValueOf(w)); rw.IsValid() && rw.Kind() == reflect.Struct {
                     if rf := rw.FieldByName("status"); rf.IsValid() && rf.Kind() == reflect.Int {
                         status = rf.Int()
                     }
                      if rf := rw.FieldByName("written"); rf.IsValid() && rf.Kind() == reflect.Int64 {
                          written = rf.Int()
                     }
                 }
                 httpl.Debugf("http: %s %q: status %d, %d bytes in %.02f ms", r.Method, r.URL.String(), status, written, ms)
             }
         })
     }

Building the file again shouldn't return any errors, but the functions are empty.

Each method takes two parameters, w and r, and they're types http.ResponseWriter and *http.Request respectively. These two parameters are populated once they're hit by a request.

## Postman
Postman is a powerful HTTP client for testing web services. Postman makes it easy to test, develop and document APIs by allowing users to quickly put together both simple and complex HTTP requests. Postman is available as both a Google Chrome Packaged App and a Google Chrome in-browser app. The packaged app version includes advanced features such as OAuth 2.0 support and bulk uploading/importing that are not available in the in-browser version. The in-browser version includes a few features, such assession cookies support, that are not yet available in the packaged app version. At publication time, the Postman REST Client is one of the highest rated productivity apps in the Chrome Web Store, with more than 348,000 unique users (for both versions), and more than 63,000 collections shared via Postman.

Postman has a very clean and intuitive user interface, with most key features accessible within one click. The learning curve for using the program is very low; most users should be able to start building and testing API calls very quickly. One big reason for Postman’s ease of use is its automation capabilities as it helps to automate the process of making API requests and testing API responses, allowing developers to establish a very efficient workflow.

![History](https://github.com/goyalharsh08/First/blob/master/history.png) 

All API calls sent using the Postman app are stored in history (the calls are displayed in the left sidebar), allowing them to be easily loaded into the response viewer at a later time. Prior API calls can be loaded into the response viewer by simply clicking the API call in the history list. Auto-complete suggestions are conveniently displayed in drop-down menus in many places throughout the app, including URL input fields, header fields and header presets. These features save developers time by eliminating the need to retype entire API calls or other pertinent API information.

![Error](https://github.com/goyalharsh08/First/blob/master/Error.png)

There might be cases when the dAppBox API doesn’t work,or exhibits unexpected behaviour. If you are not getting any response, Postman will display a message about an error in connecting to the server. 

If Postman is unable to connect to your server, it shows the message above. Usually, the easiest way to check if there are connectivity issues is to open your server address in a browser, such as Chrome or Firefox. If opening it in the browser works, then the possible causes could be:


1. Firewall Issues
2. Proxy Configurations
3. SSL Certificate Issues
4. Client Certificate Issues
5. Incorrect Request URLs
6. Using Incorrect Protocols
7. Invalid Postman Behaviour
8. Very Short Time Outs
9. Invalid Responses etc.

**Firewall issues**

Some firewalls may be configured to block non-browser connections. In this case, you should talk to your network administrators for Postman to work.

**Proxy Configuration**

If you are using a proxy server to make requests, make sure you configure it correctly. By default, Postman uses the proxy settings configured in your Operating System’s network settings. Postman Console will provide debug information about the proxy server.

**SSL Certificate issues**

When using HTTPS connections, Postman might show the error above. In this case, you can turn off SSL verification in the Postman Settings. If that does not help, your server might be using a client-side SSL connection, which you can configure in Postman Settings. Use the Postman Console to ensure that the correct SSL certificate is being sent to the server.

**Client Certificate issues**

Client certificates might be required for this server. Fix this by adding a client certificate in the Postman Settings.

**Incorrect Request URLs**

If you use variables in your request, make sure they are defined in your environment or globals. Unresolved request variables may result in invalid server addresses.

**Using incorrect protocol**

Check whether you’re accidentally using “https://” instead of “http://” in your URL (or vice versa).

**Invalid Postman behavior**

Very rarely, it is possible that Postman might be making invalid requests to your API server. You can confirm this by checking your server logs (if available). We’re always watching out for these cases, so get in touch with us if you believe Postman is misbehaving. Let us know on our GitHub issue tracker if you feel that Postman is not working as intended.

**Very short timeouts**

If you configure a very short timeout in Postman, the request might timeout before completion, resulting in the error block above. Try increasing the timeout to avoid this issue.

**Invalid Responses**

If your server sends incorrect response encoding errors, or invalid headers, Postman will fail to interpret the response, causing the error above.

If you still can’t get your API working, help can frequently be found in the Postman community or Stack Overflow.
If you’ve tried unsuccessfully troubleshooting the issue, search the Postman issue tracker on GitHub to check if someone has already reported the issue and whether there is a known solution that you can use. If you’re reporting a new issue, follow these guidelines. If you wish to include confidential data, you can file a ticket via our support center and include the app’s console logs in your report to provide some helpful data for troubleshooting.


There are also few HTTP status codes that you’ll face while testing the REST API. Here is aa list of all HTTP status codes:


**200 OK**
 
Standard response for successful HTTP requests. The actual response will depend on the request method used. In a GET request, the response will contain an entity corresponding to the requested resource. In a POST request, the response will contain an entity describing or containing the result of the action.

**201 Created**

The request has been fulfilled and resulted in a new resource being created.

**204 No Content**

The server successfully processed the request, but is not returning any content. Usually used as a response to a successful delete request.

**2xx Success**

This class of status codes indicates the action requested by the client was received, understood, accepted and processed successfully.

**400 Bad Request**

The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing)

**401 Unauthorized**

Similar to 403 Forbidden, but specifically for use when authentication is required and has failed or has not yet been provided. The response must include a WWW-Authenticate header field containing a challenge applicable to the requested resource.

**403 Forbidden**

The request was a valid request, but the server is refusing to respond to it. Unlike a 401 Unauthorized response, authenticating will make no difference.

**404 Not Found**

The requested resource could not be found but may be available again in the future. Subsequent requests by the client are permissible.

**405 Method Not Allowed**

A request method is not supported for the requested resource; for example, a GET request on a form which requires data to be presented via POST, or a PUT request on a read-only resource.

**409 Conflict**

Indicates that the request could not be processed because of conflict in the request, such as an edit conflict in the case of multiple updates.

**4xx Client Error**

The 4xx class of status code is intended for cases in which the client seems to have errored.

**500 Internal Server Error**

The server failed to fulfill an apparently valid request.

**5xx Server Error**

The server failed to fulfill an apparently valid request.


![tests](https://github.com/goyalharsh08/First/blob/master/testcase.png)

With Postman you can write and run tests for each request using the JavaScript language.

A Postman test is essentially JavaScript code executed after the request is sent, allowing access to the pm.response object.

Postman provides an environment in which tests can be written and run without any additional setup.

The tests are basically JavaScript code snippets that can be used to test API responses, ensuring that they have met the conditions as specified in the test code. A list of commonly used test snippets is displayed on the right side of the text editor so that users can add tests to API requests with just one click. The test snippets available from within Postman include (but are certainly not limited to) checking if the response body contains a string, checking if response time is less than 200 ms, checking if status code is 200 and using Tiny Validator for JSON data.

In the response box under the tests tab, those tests that have TRUE value shows PASS with the array name written or else FAIL is shown.

![test_result](https://github.com/goyalharsh08/First/blob/master/result.png)

Results are displayed in a Test tab under the response viewer. The tab header shows how many tests passed, and the test results are listed here. If the test evaluates to true, the test passed.

Test scripts are run after a request is sent and a response has been received from the server.

The older style of writing Postman relies on setting values for the special tests object. You can set a descriptive key for an element in the object and then say if it’s true or false. For example, `tests[“Body matches string”] = responsebody.has(“Syncthing”);` will check whether the response body contains the Syncthing string.

In Postman one can write and run the tests for each requests using the JavaScript language. Code added under the Tests tab will be executed after response is received.

For Example:

`tests[“Status code is 200”] = responseCode.code === 200;`

will check whether the response code received is 200.

You can have as many tests as you want for a request. Most tests are as simple and one liner JavaScript statements. Below are some more examples for the same.

Check if response body contains a string:

`tests[“Body matches string”] = responseBody.has(“string_you_want_to_search”);`

Check if response body is equal to a particular string:

`tests[“Body is correct”] = responseBody === “response_body_string”;`

Check for a JSON value:

`var data = JSON.parse(responseBody);`

`tests[“Your test name”] = data.value === 100;`

Check for Response time is less than 200ms:

`tests[“Response time is less than 200ms”] = responseTime < 200;`

Check for Successful POST request status code:

`tests[“Successful POST request”] = responseCode.code === 201 || responseCode.code === 202;`

Check for Response header content type:

`tests[“The Content-Type is JSON”] = postman.getResponseHeader(‘Content-Type’) === ‘application/json’;`

You can also write your own custom tests in JavaScript. In addition to supporting the older style of writing tests. Postman has a newer PM API (known as the pm.*API) which is the more powerful way to write tests.

Example using pm.response.to.have:

     pm.test(“Response is ok”, function () {
     pm.response.to.have.status(200);
     });

Example using response assertions

     pm.test(“response should be okay to process”, function () {
     pm.response.to.not.be.error;
     pm.response.to.have.jsonBody(“”);
     pm.response.to.not.have.jsonBody(“error”);
     });


Example using pm.response.to.be*

     pm.test(“response must be valid and have a body”, function () {
     //assert that the status code is 200
     pm.response.to.be.ok; // info, success, redirection, clientError, serverError, are other variants
     // assert that the response has a valid JSON body
     pm.response.to.be.withBody;
     pm.response.to.be.json; // this assertion also checks if a body exists, so the above check is not needed
     });

You can add as many keys as needed, depending on how many things you want to test for. Under the Test tab under the response viewer, you can view your test results. The tab header shows how many tests passed, and the keys that you set in the tests variable are listed here. If the value evaluates to true, the test passed, and if the value evaluates to be false, the test failed.

Environment and global variables can be set within JavaScript, which allows requests to be chained together. Users can import a CSV or JSON file that can then be used as mock data when running tests. Test results can be viewed in the tests tab of the response viewer, as well as in the Postman collection runner. This features allows developers to test API requests and complex scenarios without having to write a lot of additional code. Indeed, developers will likely find that Postman significantly cuts down on the amount of code they need to write themselves. This will not only save a lot of time, but will also help developers new to API testing to quickly understand how API requests work.








