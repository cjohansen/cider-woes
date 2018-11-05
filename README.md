# CIDER woes

This reproduces a problem with CIDER:

1. Open deps.edn in Emacs
2. Run `cider-jack-in-cljs`

After some time, you will observe that:

1. The node process starts up, as per `target/node/dev/node.log`
2. Figwheel is running: tail the above log, edit `src/woes/dev.cljs`, and save it
3. There is no CIDER REPL to be seen

## Environment

```
GNU Emacs 26.1 (build 1, x86_64-apple-darwin14.5.0, NS appkit-1348.17 Version 10.10.5 (Build 14F2511))
 of 2018-05-31
```

```
cider-20180903.2111
```

## Details

The last message from nREPL in `*messages*` is:

```
[nREPL] Starting server via /usr/local/bin/clojure -A:dev -Sdeps '{:deps {org.clojure/tools.nrepl {:mvn/version "0.2.13"} cider/piggieback {:mvn/version "0.3.9"} refactor-nrepl {:mvn/version "2.4.0"} cider/cider-nrepl {:mvn/version "0.18.0"}}}' -e '(require (quote cider-nrepl.main)) (cider-nrepl.main/init ["refactor-nrepl.middleware/wrap-refactor", "cider.nrepl/cider-middleware", "cider.piggieback/wrap-cljs-repl"])'...
```

If you try to jack in a second time, some more details emerge. First, the jack
in process prompts you with:

```
Buffer " *nrepl-server tmp/cider-woes:localhost*" has a running process; kill it? (y or n)
```

The buffer it's referring to was not visible in the buffer list prior to the
second jack-in. It is not visible after the second jack-in either. Responding
`y` to this prompts the following failure:

```
Buffer " *nrepl-server tmp/cider-woes:localhost*" has a running process; kill it? (y or n) y
error in process sentinel: nrepl-server-sentinel: Could not start nREPL server: 2018-11-05 08:49:06.655:INFO::main: Logging initialized @6004ms
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 1.199 seconds.
[Figwheel] Watching paths: ("src") to compile build - dev
[Figwheel] Starting Server at http://localhost:9500
[Figwheel] Starting REPL
Prompt will show when REPL connects to evaluation environment (i.e. Node)
Figwheel Main Controls:
          (figwheel.main/stop-builds id ...)  ;; stops Figwheel autobuilder for ids
          (figwheel.main/start-builds id ...) ;; starts autobuilder focused on ids
          (figwheel.main/reset)               ;; stops, cleans, reloads config, and starts autobuilder
          (figwheel.main/build-once id ...)   ;; builds source one time
          (figwheel.main/clean id ...)        ;; deletes compiled cljs target files
          (figwheel.main/status)              ;; displays current state of system
Figwheel REPL Controls:
          (figwheel.repl/conns)               ;; displays the current connections
          (figwheel.repl/focus session-name)  ;; choose which session name to focus on
In the cljs.user ns, controls can be called without ns ie. (conns) instead of (figwheel.repl/conns)
    Docs: (doc function-name-here)
    Exit: :cljs/quit
 Results: Stored in vars *1, *2, *3, *e holds last exception object
Starting node ...
Node output being logged to: target/node/dev/node.log
For a better development experience:
  1. Open chrome://inspect/#devices ... (in Chrome)
  2. Click "Open dedicated DevTools for Node"
ClojureScript 1.10.339
cljs.user=> [Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.296 seconds.
Hello world
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
Hello world
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.256 seconds.
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
Hello world
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.218 seconds.
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.576 seconds.
Hello world
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.208 seconds.
Hello world
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.192 seconds.
2018-11-05 08:55:16.112:INFO::main: Logging initialized @5741ms
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 1.097 seconds.
[Figwheel] Watching paths: ("src") to compile build - dev
[Figwheel] Starting Server at http://localhost:9500
[Figwheel] Starting REPL
Prompt will show when REPL connects to evaluation environment (i.e. Node)
Figwheel Main Controls:
          (figwheel.main/stop-builds id ...)  ;; stops Figwheel autobuilder for ids
          (figwheel.main/start-builds id ...) ;; starts autobuilder focused on ids
          (figwheel.main/reset)               ;; stops, cleans, reloads config, and starts autobuilder
          (figwheel.main/build-once id ...)   ;; builds source one time
          (figwheel.main/clean id ...)        ;; deletes compiled cljs target files
          (figwheel.main/status)              ;; displays current state of system
Figwheel REPL Controls:
          (figwheel.repl/conns)               ;; displays the current connections
          (figwheel.repl/focus session-name)  ;; choose which session name to focus on
In the cljs.user ns, controls can be called without ns ie. (conns) instead of (figwheel.repl/conns)
    Docs: (doc function-name-here)
    Exit: :cljs/quit
 Results: Stored in vars *1, *2, *3, *e holds last exception object
2018-11-05 08:55:20.498:WARN:oejuc.AbstractLifeCycle:main: FAILED ServerConnector@3bfead8d{HTTP/1.1}{0.0.0.0:9500}: java.net.BindException: Address already in use
java.net.BindException: Address already in use
    at sun.nio.ch.Net.bind0(Native Method)
    at sun.nio.ch.Net.bind(Net.java:433)
    at sun.nio.ch.Net.bind(Net.java:425)
    at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
    at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
    at org.eclipse.jetty.server.ServerConnector.open(ServerConnector.java:321)
    at org.eclipse.jetty.server.AbstractNetworkConnector.doStart(AbstractNetworkConnector.java:80)
    at org.eclipse.jetty.server.ServerConnector.doStart(ServerConnector.java:236)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at org.eclipse.jetty.server.Server.doStart(Server.java:366)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at ring.adapter.jetty$run_jetty.invokeStatic(jetty.clj:171)
    at ring.adapter.jetty$run_jetty.invoke(jetty.clj:127)
    at figwheel.server.jetty_websocket$run_jetty.invokeStatic(jetty_websocket.clj:126)
    at figwheel.server.jetty_websocket$run_jetty.invoke(jetty_websocket.clj:125)
    at figwheel.server.jetty_websocket$run_server.invokeStatic(jetty_websocket.clj:179)
    at figwheel.server.jetty_websocket$run_server.invoke(jetty_websocket.clj:178)
    at figwheel.repl$run_default_server_STAR_.invokeStatic(repl.cljc:1098)
    at figwheel.repl$run_default_server_STAR_.invoke(repl.cljc:1091)
    at figwheel.repl$run_default_server.invokeStatic(repl.cljc:1131)
    at figwheel.repl$run_default_server.invoke(repl.cljc:1130)
    at figwheel.repl$setup.invokeStatic(repl.cljc:1200)
    at figwheel.repl$setup.invoke(repl.cljc:1195)
    at figwheel.repl.FigwheelReplEnv._setup(repl.cljc:1301)
    at cljs.repl$repl_STAR_$fn__6607.invoke(repl.cljc:942)
    at cljs.compiler$with_core_cljs.invokeStatic(compiler.cljc:1289)
    at cljs.compiler$with_core_cljs.invoke(compiler.cljc:1278)
    at cljs.repl$repl_STAR_.invokeStatic(repl.cljc:940)
    at cljs.repl$repl_STAR_.invoke(repl.cljc:855)
    at figwheel.main$repl.invokeStatic(main.cljc:1565)
    at figwheel.main$repl.invoke(main.cljc:1522)
    at figwheel.main$repl_action.invokeStatic(main.cljc:1577)
    at figwheel.main$repl_action.invoke(main.cljc:1572)
    at figwheel.main$default_compile.invokeStatic(main.cljc:1852)
    at figwheel.main$default_compile.invoke(main.cljc:1812)
    at figwheel.main$build_main_opt.invokeStatic(main.cljc:526)
    at figwheel.main$build_main_opt.invoke(main.cljc:521)
    at cljs.cli$main.invokeStatic(cli.clj:636)
    at cljs.cli$main.doInvoke(cli.clj:625)
    at clojure.lang.RestFn.applyTo(RestFn.java:139)
    at clojure.core$apply.invokeStatic(core.clj:659)
    at clojure.core$apply.invoke(core.clj:652)
    at cljs.main$_main.invokeStatic(main.clj:61)
    at cljs.main$_main.doInvoke(main.clj:52)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.core$apply.invoke(core.clj:652)
    at figwheel.main$_main$fn__7575.invoke(main.cljc:2165)
    at clojure.core$with_redefs_fn.invokeStatic(core.clj:7434)
    at clojure.core$with_redefs_fn.invoke(core.clj:7418)
    at figwheel.main$_main.invokeStatic(main.cljc:2163)
    at figwheel.main$_main.doInvoke(main.cljc:2142)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.main$main_opt.invokeStatic(main.clj:317)
    at clojure.main$main_opt.invoke(main.clj:313)
    at clojure.main$main.invokeStatic(main.clj:424)
    at clojure.main$main.doInvoke(main.clj:387)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.main.main(main.java:37)
2018-11-05 08:55:20.652:WARN:oejuc.AbstractLifeCycle:main: FAILED org.eclipse.jetty.server.Server@5799b8a2: java.net.BindException: Address already in use
java.net.BindException: Address already in use
    at sun.nio.ch.Net.bind0(Native Method)
    at sun.nio.ch.Net.bind(Net.java:433)
    at sun.nio.ch.Net.bind(Net.java:425)
    at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
    at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
    at org.eclipse.jetty.server.ServerConnector.open(ServerConnector.java:321)
    at org.eclipse.jetty.server.AbstractNetworkConnector.doStart(AbstractNetworkConnector.java:80)
    at org.eclipse.jetty.server.ServerConnector.doStart(ServerConnector.java:236)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at org.eclipse.jetty.server.Server.doStart(Server.java:366)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at ring.adapter.jetty$run_jetty.invokeStatic(jetty.clj:171)
    at ring.adapter.jetty$run_jetty.invoke(jetty.clj:127)
    at figwheel.server.jetty_websocket$run_jetty.invokeStatic(jetty_websocket.clj:126)
    at figwheel.server.jetty_websocket$run_jetty.invoke(jetty_websocket.clj:125)
    at figwheel.server.jetty_websocket$run_server.invokeStatic(jetty_websocket.clj:179)
    at figwheel.server.jetty_websocket$run_server.invoke(jetty_websocket.clj:178)
    at figwheel.repl$run_default_server_STAR_.invokeStatic(repl.cljc:1098)
    at figwheel.repl$run_default_server_STAR_.invoke(repl.cljc:1091)
    at figwheel.repl$run_default_server.invokeStatic(repl.cljc:1131)
    at figwheel.repl$run_default_server.invoke(repl.cljc:1130)
    at figwheel.repl$setup.invokeStatic(repl.cljc:1200)
    at figwheel.repl$setup.invoke(repl.cljc:1195)
    at figwheel.repl.FigwheelReplEnv._setup(repl.cljc:1301)
    at cljs.repl$repl_STAR_$fn__6607.invoke(repl.cljc:942)
    at cljs.compiler$with_core_cljs.invokeStatic(compiler.cljc:1289)
    at cljs.compiler$with_core_cljs.invoke(compiler.cljc:1278)
    at cljs.repl$repl_STAR_.invokeStatic(repl.cljc:940)
    at cljs.repl$repl_STAR_.invoke(repl.cljc:855)
    at figwheel.main$repl.invokeStatic(main.cljc:1565)
    at figwheel.main$repl.invoke(main.cljc:1522)
    at figwheel.main$repl_action.invokeStatic(main.cljc:1577)
    at figwheel.main$repl_action.invoke(main.cljc:1572)
    at figwheel.main$default_compile.invokeStatic(main.cljc:1852)
    at figwheel.main$default_compile.invoke(main.cljc:1812)
    at figwheel.main$build_main_opt.invokeStatic(main.cljc:526)
    at figwheel.main$build_main_opt.invoke(main.cljc:521)
    at cljs.cli$main.invokeStatic(cli.clj:636)
    at cljs.cli$main.doInvoke(cli.clj:625)
    at clojure.lang.RestFn.applyTo(RestFn.java:139)
    at clojure.core$apply.invokeStatic(core.clj:659)
    at clojure.core$apply.invoke(core.clj:652)
    at cljs.main$_main.invokeStatic(main.clj:61)
    at cljs.main$_main.doInvoke(main.clj:52)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.core$apply.invoke(core.clj:652)
    at figwheel.main$_main$fn__7575.invoke(main.cljc:2165)
    at clojure.core$with_redefs_fn.invokeStatic(core.clj:7434)
    at clojure.core$with_redefs_fn.invoke(core.clj:7418)
    at figwheel.main$_main.invokeStatic(main.cljc:2163)
    at figwheel.main$_main.doInvoke(main.cljc:2142)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.main$main_opt.invokeStatic(main.clj:317)
    at clojure.main$main_opt.invoke(main.clj:313)
    at clojure.main$main.invokeStatic(main.clj:424)
    at clojure.main$main.doInvoke(main.clj:387)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.main.main(main.java:37)
Exception in thread "main" java.net.BindException: Address already in use
    at sun.nio.ch.Net.bind0(Native Method)
    at sun.nio.ch.Net.bind(Net.java:433)
    at sun.nio.ch.Net.bind(Net.java:425)
    at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
    at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
    at org.eclipse.jetty.server.ServerConnector.open(ServerConnector.java:321)
    at org.eclipse.jetty.server.AbstractNetworkConnector.doStart(AbstractNetworkConnector.java:80)
    at org.eclipse.jetty.server.ServerConnector.doStart(ServerConnector.java:236)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at org.eclipse.jetty.server.Server.doStart(Server.java:366)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at ring.adapter.jetty$run_jetty.invokeStatic(jetty.clj:171)
    at ring.adapter.jetty$run_jetty.invoke(jetty.clj:127)
    at figwheel.server.jetty_websocket$run_jetty.invokeStatic(jetty_websocket.clj:126)
    at figwheel.server.jetty_websocket$run_jetty.invoke(jetty_websocket.clj:125)
    at figwheel.server.jetty_websocket$run_server.invokeStatic(jetty_websocket.clj:179)
    at figwheel.server.jetty_websocket$run_server.invoke(jetty_websocket.clj:178)
    at figwheel.repl$run_default_server_STAR_.invokeStatic(repl.cljc:1098)
    at figwheel.repl$run_default_server_STAR_.invoke(repl.cljc:1091)
    at figwheel.repl$run_default_server.invokeStatic(repl.cljc:1131)
    at figwheel.repl$run_default_server.invoke(repl.cljc:1130)
    at figwheel.repl$setup.invokeStatic(repl.cljc:1200)
    at figwheel.repl$setup.invoke(repl.cljc:1195)
    at figwheel.repl.FigwheelReplEnv._setup(repl.cljc:1301)
    at cljs.repl$repl_STAR_$fn__6607.invoke(repl.cljc:942)
    at cljs.compiler$with_core_cljs.invokeStatic(compiler.cljc:1289)
    at cljs.compiler$with_core_cljs.invoke(compiler.cljc:1278)
    at cljs.repl$repl_STAR_.invokeStatic(repl.cljc:940)
    at cljs.repl$repl_STAR_.invoke(repl.cljc:855)
    at figwheel.main$repl.invokeStatic(main.cljc:1565)
    at figwheel.main$repl.invoke(main.cljc:1522)
    at figwheel.main$repl_action.invokeStatic(main.cljc:1577)
    at figwheel.main$repl_action.invoke(main.cljc:1572)
    at figwheel.main$default_compile.invokeStatic(main.cljc:1852)
    at figwheel.main$default_compile.invoke(main.cljc:1812)
    at figwheel.main$build_main_opt.invokeStatic(main.cljc:526)
    at figwheel.main$build_main_opt.invoke(main.cljc:521)
    at cljs.cli$main.invokeStatic(cli.clj:636)
    at cljs.cli$main.doInvoke(cli.clj:625)
    at clojure.lang.RestFn.applyTo(RestFn.java:139)
    at clojure.core$apply.invokeStatic(core.clj:659)
    at clojure.core$apply.invoke(core.clj:652)
    at cljs.main$_main.invokeStatic(main.clj:61)
    at cljs.main$_main.doInvoke(main.clj:52)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.core$apply.invoke(core.clj:652)
    at figwheel.main$_main$fn__7575.invoke(main.cljc:2165)
    at clojure.core$with_redefs_fn.invokeStatic(core.clj:7434)
    at clojure.core$with_redefs_fn.invoke(core.clj:7418)
    at figwheel.main$_main.invokeStatic(main.cljc:2163)
    at figwheel.main$_main.doInvoke(main.cljc:2142)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.main$main_opt.invokeStatic(main.clj:317)
    at clojure.main$main_opt.invoke(main.clj:313)
    at clojure.main$main.invokeStatic(main.clj:424)
    at clojure.main$main.doInvoke(main.clj:387)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.main.main(main.java:37)

error in process sentinel: Could not start nREPL server: 2018-11-05 08:49:06.655:INFO::main: Logging initialized @6004ms
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 1.199 seconds.
[Figwheel] Watching paths: ("src") to compile build - dev
[Figwheel] Starting Server at http://localhost:9500
[Figwheel] Starting REPL
Prompt will show when REPL connects to evaluation environment (i.e. Node)
Figwheel Main Controls:
          (figwheel.main/stop-builds id ...)  ;; stops Figwheel autobuilder for ids
          (figwheel.main/start-builds id ...) ;; starts autobuilder focused on ids
          (figwheel.main/reset)               ;; stops, cleans, reloads config, and starts autobuilder
          (figwheel.main/build-once id ...)   ;; builds source one time
          (figwheel.main/clean id ...)        ;; deletes compiled cljs target files
          (figwheel.main/status)              ;; displays current state of system
Figwheel REPL Controls:
          (figwheel.repl/conns)               ;; displays the current connections
          (figwheel.repl/focus session-name)  ;; choose which session name to focus on
In the cljs.user ns, controls can be called without ns ie. (conns) instead of (figwheel.repl/conns)
    Docs: (doc function-name-here)
    Exit: :cljs/quit
 Results: Stored in vars *1, *2, *3, *e holds last exception object
Starting node ...
Node output being logged to: target/node/dev/node.log
For a better development experience:
  1. Open chrome://inspect/#devices ... (in Chrome)
  2. Click "Open dedicated DevTools for Node"
ClojureScript 1.10.339
cljs.user=> [Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.296 seconds.
Hello world
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
Hello world
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.256 seconds.
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
Hello world
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.218 seconds.
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.576 seconds.
Hello world
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.208 seconds.
Hello world
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 0.192 seconds.
2018-11-05 08:55:16.112:INFO::main: Logging initialized @5741ms
[Figwheel] Compiling build dev to "target/node/dev/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/node/dev/dev-main.js" in 1.097 seconds.
[Figwheel] Watching paths: ("src") to compile build - dev
[Figwheel] Starting Server at http://localhost:9500
[Figwheel] Starting REPL
Prompt will show when REPL connects to evaluation environment (i.e. Node)
Figwheel Main Controls:
          (figwheel.main/stop-builds id ...)  ;; stops Figwheel autobuilder for ids
          (figwheel.main/start-builds id ...) ;; starts autobuilder focused on ids
          (figwheel.main/reset)               ;; stops, cleans, reloads config, and starts autobuilder
          (figwheel.main/build-once id ...)   ;; builds source one time
          (figwheel.main/clean id ...)        ;; deletes compiled cljs target files
          (figwheel.main/status)              ;; displays current state of system
Figwheel REPL Controls:
          (figwheel.repl/conns)               ;; displays the current connections
          (figwheel.repl/focus session-name)  ;; choose which session name to focus on
In the cljs.user ns, controls can be called without ns ie. (conns) instead of (figwheel.repl/conns)
    Docs: (doc function-name-here)
    Exit: :cljs/quit
 Results: Stored in vars *1, *2, *3, *e holds last exception object
2018-11-05 08:55:20.498:WARN:oejuc.AbstractLifeCycle:main: FAILED ServerConnector@3bfead8d{HTTP/1.1}{0.0.0.0:9500}: java.net.BindException: Address already in use
java.net.BindException: Address already in use
    at sun.nio.ch.Net.bind0(Native Method)
    at sun.nio.ch.Net.bind(Net.java:433)
    at sun.nio.ch.Net.bind(Net.java:425)
    at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
    at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
    at org.eclipse.jetty.server.ServerConnector.open(ServerConnector.java:321)
    at org.eclipse.jetty.server.AbstractNetworkConnector.doStart(AbstractNetworkConnector.java:80)
    at org.eclipse.jetty.server.ServerConnector.doStart(ServerConnector.java:236)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at org.eclipse.jetty.server.Server.doStart(Server.java:366)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at ring.adapter.jetty$run_jetty.invokeStatic(jetty.clj:171)
    at ring.adapter.jetty$run_jetty.invoke(jetty.clj:127)
    at figwheel.server.jetty_websocket$run_jetty.invokeStatic(jetty_websocket.clj:126)
    at figwheel.server.jetty_websocket$run_jetty.invoke(jetty_websocket.clj:125)
    at figwheel.server.jetty_websocket$run_server.invokeStatic(jetty_websocket.clj:179)
    at figwheel.server.jetty_websocket$run_server.invoke(jetty_websocket.clj:178)
    at figwheel.repl$run_default_server_STAR_.invokeStatic(repl.cljc:1098)
    at figwheel.repl$run_default_server_STAR_.invoke(repl.cljc:1091)
    at figwheel.repl$run_default_server.invokeStatic(repl.cljc:1131)
    at figwheel.repl$run_default_server.invoke(repl.cljc:1130)
    at figwheel.repl$setup.invokeStatic(repl.cljc:1200)
    at figwheel.repl$setup.invoke(repl.cljc:1195)
    at figwheel.repl.FigwheelReplEnv._setup(repl.cljc:1301)
    at cljs.repl$repl_STAR_$fn__6607.invoke(repl.cljc:942)
    at cljs.compiler$with_core_cljs.invokeStatic(compiler.cljc:1289)
    at cljs.compiler$with_core_cljs.invoke(compiler.cljc:1278)
    at cljs.repl$repl_STAR_.invokeStatic(repl.cljc:940)
    at cljs.repl$repl_STAR_.invoke(repl.cljc:855)
    at figwheel.main$repl.invokeStatic(main.cljc:1565)
    at figwheel.main$repl.invoke(main.cljc:1522)
    at figwheel.main$repl_action.invokeStatic(main.cljc:1577)
    at figwheel.main$repl_action.invoke(main.cljc:1572)
    at figwheel.main$default_compile.invokeStatic(main.cljc:1852)
    at figwheel.main$default_compile.invoke(main.cljc:1812)
    at figwheel.main$build_main_opt.invokeStatic(main.cljc:526)
    at figwheel.main$build_main_opt.invoke(main.cljc:521)
    at cljs.cli$main.invokeStatic(cli.clj:636)
    at cljs.cli$main.doInvoke(cli.clj:625)
    at clojure.lang.RestFn.applyTo(RestFn.java:139)
    at clojure.core$apply.invokeStatic(core.clj:659)
    at clojure.core$apply.invoke(core.clj:652)
    at cljs.main$_main.invokeStatic(main.clj:61)
    at cljs.main$_main.doInvoke(main.clj:52)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.core$apply.invoke(core.clj:652)
    at figwheel.main$_main$fn__7575.invoke(main.cljc:2165)
    at clojure.core$with_redefs_fn.invokeStatic(core.clj:7434)
    at clojure.core$with_redefs_fn.invoke(core.clj:7418)
    at figwheel.main$_main.invokeStatic(main.cljc:2163)
    at figwheel.main$_main.doInvoke(main.cljc:2142)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.main$main_opt.invokeStatic(main.clj:317)
    at clojure.main$main_opt.invoke(main.clj:313)
    at clojure.main$main.invokeStatic(main.clj:424)
    at clojure.main$main.doInvoke(main.clj:387)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.main.main(main.java:37)
2018-11-05 08:55:20.652:WARN:oejuc.AbstractLifeCycle:main: FAILED org.eclipse.jetty.server.Server@5799b8a2: java.net.BindException: Address already in use
java.net.BindException: Address already in use
    at sun.nio.ch.Net.bind0(Native Method)
    at sun.nio.ch.Net.bind(Net.java:433)
    at sun.nio.ch.Net.bind(Net.java:425)
    at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
    at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
    at org.eclipse.jetty.server.ServerConnector.open(ServerConnector.java:321)
    at org.eclipse.jetty.server.AbstractNetworkConnector.doStart(AbstractNetworkConnector.java:80)
    at org.eclipse.jetty.server.ServerConnector.doStart(ServerConnector.java:236)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at org.eclipse.jetty.server.Server.doStart(Server.java:366)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at ring.adapter.jetty$run_jetty.invokeStatic(jetty.clj:171)
    at ring.adapter.jetty$run_jetty.invoke(jetty.clj:127)
    at figwheel.server.jetty_websocket$run_jetty.invokeStatic(jetty_websocket.clj:126)
    at figwheel.server.jetty_websocket$run_jetty.invoke(jetty_websocket.clj:125)
    at figwheel.server.jetty_websocket$run_server.invokeStatic(jetty_websocket.clj:179)
    at figwheel.server.jetty_websocket$run_server.invoke(jetty_websocket.clj:178)
    at figwheel.repl$run_default_server_STAR_.invokeStatic(repl.cljc:1098)
    at figwheel.repl$run_default_server_STAR_.invoke(repl.cljc:1091)
    at figwheel.repl$run_default_server.invokeStatic(repl.cljc:1131)
    at figwheel.repl$run_default_server.invoke(repl.cljc:1130)
    at figwheel.repl$setup.invokeStatic(repl.cljc:1200)
    at figwheel.repl$setup.invoke(repl.cljc:1195)
    at figwheel.repl.FigwheelReplEnv._setup(repl.cljc:1301)
    at cljs.repl$repl_STAR_$fn__6607.invoke(repl.cljc:942)
    at cljs.compiler$with_core_cljs.invokeStatic(compiler.cljc:1289)
    at cljs.compiler$with_core_cljs.invoke(compiler.cljc:1278)
    at cljs.repl$repl_STAR_.invokeStatic(repl.cljc:940)
    at cljs.repl$repl_STAR_.invoke(repl.cljc:855)
    at figwheel.main$repl.invokeStatic(main.cljc:1565)
    at figwheel.main$repl.invoke(main.cljc:1522)
    at figwheel.main$repl_action.invokeStatic(main.cljc:1577)
    at figwheel.main$repl_action.invoke(main.cljc:1572)
    at figwheel.main$default_compile.invokeStatic(main.cljc:1852)
    at figwheel.main$default_compile.invoke(main.cljc:1812)
    at figwheel.main$build_main_opt.invokeStatic(main.cljc:526)
    at figwheel.main$build_main_opt.invoke(main.cljc:521)
    at cljs.cli$main.invokeStatic(cli.clj:636)
    at cljs.cli$main.doInvoke(cli.clj:625)
    at clojure.lang.RestFn.applyTo(RestFn.java:139)
    at clojure.core$apply.invokeStatic(core.clj:659)
    at clojure.core$apply.invoke(core.clj:652)
    at cljs.main$_main.invokeStatic(main.clj:61)
    at cljs.main$_main.doInvoke(main.clj:52)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.core$apply.invoke(core.clj:652)
    at figwheel.main$_main$fn__7575.invoke(main.cljc:2165)
    at clojure.core$with_redefs_fn.invokeStatic(core.clj:7434)
    at clojure.core$with_redefs_fn.invoke(core.clj:7418)
    at figwheel.main$_main.invokeStatic(main.cljc:2163)
    at figwheel.main$_main.doInvoke(main.cljc:2142)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.main$main_opt.invokeStatic(main.clj:317)
    at clojure.main$main_opt.invoke(main.clj:313)
    at clojure.main$main.invokeStatic(main.clj:424)
    at clojure.main$main.doInvoke(main.clj:387)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.main.main(main.java:37)
Exception in thread "main" java.net.BindException: Address already in use
    at sun.nio.ch.Net.bind0(Native Method)
    at sun.nio.ch.Net.bind(Net.java:433)
    at sun.nio.ch.Net.bind(Net.java:425)
    at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
    at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
    at org.eclipse.jetty.server.ServerConnector.open(ServerConnector.java:321)
    at org.eclipse.jetty.server.AbstractNetworkConnector.doStart(AbstractNetworkConnector.java:80)
    at org.eclipse.jetty.server.ServerConnector.doStart(ServerConnector.java:236)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at org.eclipse.jetty.server.Server.doStart(Server.java:366)
    at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
    at ring.adapter.jetty$run_jetty.invokeStatic(jetty.clj:171)
    at ring.adapter.jetty$run_jetty.invoke(jetty.clj:127)
    at figwheel.server.jetty_websocket$run_jetty.invokeStatic(jetty_websocket.clj:126)
    at figwheel.server.jetty_websocket$run_jetty.invoke(jetty_websocket.clj:125)
    at figwheel.server.jetty_websocket$run_server.invokeStatic(jetty_websocket.clj:179)
    at figwheel.server.jetty_websocket$run_server.invoke(jetty_websocket.clj:178)
    at figwheel.repl$run_default_server_STAR_.invokeStatic(repl.cljc:1098)
    at figwheel.repl$run_default_server_STAR_.invoke(repl.cljc:1091)
    at figwheel.repl$run_default_server.invokeStatic(repl.cljc:1131)
    at figwheel.repl$run_default_server.invoke(repl.cljc:1130)
    at figwheel.repl$setup.invokeStatic(repl.cljc:1200)
    at figwheel.repl$setup.invoke(repl.cljc:1195)
    at figwheel.repl.FigwheelReplEnv._setup(repl.cljc:1301)
    at cljs.repl$repl_STAR_$fn__6607.invoke(repl.cljc:942)
    at cljs.compiler$with_core_cljs.invokeStatic(compiler.cljc:1289)
    at cljs.compiler$with_core_cljs.invoke(compiler.cljc:1278)
    at cljs.repl$repl_STAR_.invokeStatic(repl.cljc:940)
    at cljs.repl$repl_STAR_.invoke(repl.cljc:855)
    at figwheel.main$repl.invokeStatic(main.cljc:1565)
    at figwheel.main$repl.invoke(main.cljc:1522)
    at figwheel.main$repl_action.invokeStatic(main.cljc:1577)
    at figwheel.main$repl_action.invoke(main.cljc:1572)
    at figwheel.main$default_compile.invokeStatic(main.cljc:1852)
    at figwheel.main$default_compile.invoke(main.cljc:1812)
    at figwheel.main$build_main_opt.invokeStatic(main.cljc:526)
    at figwheel.main$build_main_opt.invoke(main.cljc:521)
    at cljs.cli$main.invokeStatic(cli.clj:636)
    at cljs.cli$main.doInvoke(cli.clj:625)
    at clojure.lang.RestFn.applyTo(RestFn.java:139)
    at clojure.core$apply.invokeStatic(core.clj:659)
    at clojure.core$apply.invoke(core.clj:652)
    at cljs.main$_main.invokeStatic(main.clj:61)
    at cljs.main$_main.doInvoke(main.clj:52)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.core$apply.invoke(core.clj:652)
    at figwheel.main$_main$fn__7575.invoke(main.cljc:2165)
    at clojure.core$with_redefs_fn.invokeStatic(core.clj:7434)
    at clojure.core$with_redefs_fn.invoke(core.clj:7418)
    at figwheel.main$_main.invokeStatic(main.cljc:2163)
    at figwheel.main$_main.doInvoke(main.cljc:2142)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.core$apply.invokeStatic(core.clj:657)
    at clojure.main$main_opt.invokeStatic(main.clj:317)
    at clojure.main$main_opt.invoke(main.clj:313)
    at clojure.main$main.invokeStatic(main.clj:424)
    at clojure.main$main.doInvoke(main.clj:387)
    at clojure.lang.RestFn.applyTo(RestFn.java:137)
    at clojure.lang.Var.applyTo(Var.java:702)
    at clojure.main.main(main.java:37)

error in process sentinel: nrepl-server-sentinel: Could not start nREPL server:
error in process sentinel: Could not start nREPL server:
```
