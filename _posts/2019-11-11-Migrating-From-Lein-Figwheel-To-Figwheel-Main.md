---
layout: post
title: Migrating from lein-figwheel to figwheel-main and keeping Cursive REPL
---

`figwheel-main` is a re-write of Figwheel, that doesn't depend on `lein-cljsbuild` config and doesn't require Leiningen at all. Migrating a Leiningen project to it is not too difficult but there may be a few gotchas, depending on your project, as `figwheel-main` doesn't support all of the config options of `lein-figwheel`. Also, our specific case was to continue using Cursive nREPL integration - something we were enjoying with `lein-figwheel`.

## Testing

The nice thing about new Figwheel is that it includes a dedicated test runner so we were able to remove our `devcards` setup which we copied from [8th Light blog](https://8thlight.com/blog/eric-smith/2016/10/05/a-testable-clojurescript-setup.html). Now the tests can be executed simply by navigating to `localhost:9500/figwheel-extra-main/auto-testing` when Figwheel is running, provided that:

- the `"test"` directory is included in `:watch-dirs`
- `:auto-testing` is set to `true`

Both the `:watch-dirs` and `:auto-testing` are Figwheel config options, which we store in `dev.cljs.edn` file as a metadata map of a compiler config map. We moved the compiler config map itself from `:cljsbuild` `:dev` config in `project.clj` - more on it in the section below.

While `devcards` is useful for many things, and we enjoyed using it for running unit tests, there were a few gotchas in this use case. The main was that each namespace would have a separate 'page' so running all tests in a project at once was not obviously possible.

## No :cljsbuild config

As already mentioned, unless you have a specific need for it, most or all of `:cljsbuild` config maps, and the `lein-cljsbuild` plugin itself, can be removed from `project.clj`, because Fighweel is now able to do a minified build as well. Our config for it is in `min.cljs.edn` file which contains the same map as the `:compiler` map in now-removed `:cljsbuild` `:min` config. To run the build, type the following command, assuming you are using Leiningen:

    lein trampoline run -m figwheel.main -bo min

## binaryage/devtools included

This essential tool is now automatically included in dev Figwheel builds so the project dependency and the `:preloads` entry can be removed.

## REPL

To get Cursive ǹREPL integration we did:

- replace any `piggieback` dependency with `cider/piggieback`
- add `cider/cider-nrepl` plugin

Also, this is how our `:repl-options` map looks like:

    {:nrepl-middleware [cider.piggieback/wrap-cljs-repl]
     :init (do (require '[figwheel.main.api :as f])
               (future (f/start {:mode :serve} "dev")))
     :port 7890}

To run it, simply type:
    
    lein repl

And then, after connecting your REPL to port `7890`, just type this in the REPL:

    (f/cljs-repl "dev")

This replaces the `:nrepl-port` option from `lein-figwheel`.

## :on-jsload

The handly `:on-jsload` option, which specifies a function to call on each Figwheel refresh, is also gone. The replacement is easy though:

1. Add `^:figwheel-hooks` metadata to the `ns` containing the function.
2. Add `^:after-load` metadata to the function.

## :open-urls

Also the `:open-urls` option is gone and replaced with `:open-url` which takes `false` or a string.

## Summary

The summary is short. We are very happy with `figwheel-main`! Thank you, Bruce Hauman!
