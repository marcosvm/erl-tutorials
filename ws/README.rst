===============
WS (web server)
===============

About
-----
* A basic web server using inets service API

Dependencies
------------
- Erlang/OTP

Author
------
Ivan Ribeiro Rocha <ivan.ribeiro@gmail.com> 

=======
Testing
=======

From inside **ws1/src** directory, type::

 erl -pa ../ebin +K true +A 42 +B -s inets start -run ws_app start

To change parameters (*module, timeout or port*), use (**-ws <parameter> <value>**)::

 erl -pa ../ebin +K true +A 42 +B -s inets start -run ws_app start -ws <parameter> <value>

Valid parameters can be checked inside **ws1/ebin/ws.app**, see bellow::

 {application, ws,
  [{description, "web server application"},
   {vsn, "1.0"},
   {modules, [ws_app, ws_sup, ws]},
   {registered, [ws]},
   {applications, [kernel, stdlib, inets]},
   {mod, {ws_app, []}},
   {env, [{modules, [ws]}, {port, 1972}, {timeout, infinity}, 
          {bind, {127,0,0,1}}, {name, "ws1"},
          {server_root, "/tmp"}, {document_root, "/tmp"}]}
  ]}.

Making a request::

 [irocha@napoleon ~]$ curl -v http://localhost:1972/ -d "data=ale%20&%20ivan";echo
 * About to connect() to localhost port 1972 (#0)
 *   Trying 127.0.0.1... connected
 * Connected to localhost (127.0.0.1) port 1972 (#0)
 > POST / HTTP/1.1
 > User-Agent: curl/7.20.1 (x86_64-redhat-linux-gnu) libcurl/7.20.1 NSS/3.12.6.2 zlib/1.2.3 libidn/1.16 libssh2/1.2.4
 > Host: localhost:1972
 > Accept: */*
 > Content-Length: 19
 > Content-Type: application/x-www-form-urlencoded
 > 
 < HTTP/1.1 200 OK
 < Server: inets/5.3
 < Date: Sat, 19 Jun 2010 20:44:54 GMT
 < Content-Length: 544
 < Content-Type: plain/text; charset=ISO-8859-1
 < 
 WS (data received):
 {{mod,{init_data,{51378,"127.0.0.1"},"napoleon"},
       [],ip_comm,#Port<0.1095>,httpd_conf__127_0_0_1__1972,"POST",
       "localhost:1972/","/","HTTP/1.1","POST / HTTP/1.1",
       [{"content-type","application/x-www-form-urlencoded"},
        {"content-length","19"},
        {"accept","*/*"},
        {"host","localhost:1972"},
        {"user-agent",
         "curl/7.20.1 (x86_64-redhat-linux-gnu) libcurl/7.20.1 NSS/3.12.6.2 zlib/1.2.3 libidn/1.16 libssh2/1.2.4"}],
       "data=ale%20&%20ivan",true},
  "data=ale%20&%20ivan"}
 * Connection #0 to host localhost left intact
 * Closing connection #0

========
Releases
========

Rel file::

 {release,
  {"ws_rel", "A"},
  {erts, "5.8"},
  [{kernel, "2.14"},
   {stdlib, "1.17"},
   {inets, "5.3"},
   {ws, "1.0"}]
 }.

Generating *boot scripts* from inside **ws1/src** directory::

 [irocha@napoleon src (master)]$ erl -pa ../ebin +K true +A 42 +B
 Erlang R14A (erts-5.8) [source] [64-bit] [smp:2:2] [rq:2] [async-threads:42] [hipe] [kernel-poll:true]

 Eshell V5.8  (abort with ^G)
 1> systools:make_script("ws_rel-1.0", [local]).
 ok

* **local** is an option that means that the directories where the applications are found are used in the *boot script*, instead of $ROOT/lib
* **$ROOT** is the root directory of the installed release

If you want to make a **release package**, type::

 [irocha@napoleon src (master)]$ erl -pa ../ebin +K true +A 42 +B
 Erlang R14A (erts-5.8) [source] [64-bit] [smp:2:2] [rq:2] [async-threads:42] [hipe] [kernel-poll:true]

 Eshell V5.8  (abort with ^G)
 1> systools:make_script("ws_rel-1.0").
 ok
 2> systools:make_tar("ws_rel-1.0").   
 ok

Generated files::
 
 ws_rel-1.0.boot
 ws_rel-1.0.script
 ws_rel-1.0.tar.gz (make_tar)

Executing **ws1** *boot script*::

 [irocha@napoleon src (master)]$ erl +K true +A 42 +B -boot ws_rel-1.0
 Erlang R14A (erts-5.8) [source] [64-bit] [smp:2:2] [rq:2] [async-threads:42] [hipe] [kernel-poll:true]

 WS started [{port,1972},
             {server_root,"/tmp"},
             {document_root,"/tmp"},
             {bind_address,{127,0,0,1}},
             {server_name,"ws1"},
             {modules,[ws]}]...

 Eshell V5.8  (abort with ^G)
 1> 
 

 

 



