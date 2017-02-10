# raven-clj

A thin wrapper around the
[official Java library for Sentry](https://github.com/getsentry/raven-java/).

## Usage

```clojure
(require '[raven-clj.core :as raven])

(def dsn
  "https://blah:blee@sentry.io/bloo")

(try
  (do-something-risky)
  (catch Exception e
    (raven/send-event dsn {:throwable e})))
```

## Supported event keys

- `:breadcrumbs` - a collection of `Breadcrumb` maps. See below.
- `:checksum-for` - a `String` whose checksum should be associated with the event.
- `:checksum` - a `String` which _is_ the checksum for the event, if you're doing your own checksumming.
- `:culprit` - a `String` or a `StackTraceElement` which identifies the so-called culprit. This is not very well documented at Sentry, but [here is an example](https://docs.sentry.io/clients/ruby/integrations/puma/) of how they use the `String` variant in a Ruby application.
- `:environment` - a `String` which identifies the environment.
- `:event-id` - a `UUID` to identify the event. Autogenerated if not specified.
- `:extra` - a map with `Keyword` or `String` keys (or anything for which `clojure.core/name` can be invoked) and values which can be JSON-ified. If `:throwable` is given, this will automatically include its `ex-data`.
- `:fingerprint` - a sequence of `String`s that Sentry should use as a [fingerprint](https://docs.sentry.io/learn/rollups/#customize-grouping-with-fingerprints).
- `:interfaces` - a map with `Keyword` or `String` keys and values that can be JSON-ified. See the [Sentry documentation on interfaces](https://docs.sentry.io/clientdev/interfaces/) for information on the expected format.
- `:level` - a `Keyword`. One of `:debug`, `:info`, `:warning`, `:error`, `:fatal`. Probably most useful in conjunction with `:message` if you need to report an exceptional condition that's not an exception.
- `:logger` - a `String` which identifies the logger.
- `:message` - a `String`. This is a different message than that of a Throwable.
- `:platform` - a `String` which identifies the platform.
- `:release` - a `String` which identifies the release.
- `:server-name` - a `String` which identifies the server name.
- `:tags` - a map with `Keyword` or `String` keys (or anything for which `clojure.core/name` can be invoked) and values which can be coerced to `Strings` with `clojure.core/str`.
- `:throwable` - a `Throwable` object. Sentry's bread and butter.
- `:timestamp` - something that can be coerced to an `inst` using [clj-time's `to-date` coercion](https://github.com/clj-time/clj-time#clj-timecoerce)

### Breadcrumbs

Read more about breadcrumbs at [docs.sentry.io/learn/breadcrumbs/](https://docs.sentry.io/learn/breadcrumbs/).

When an event has a `:breadcrumbs` key, each element of the value collection should be a map.
Supported keys of the map are:

- `:type` - A `String`
- `:timestamp` - something that can be coerced to an `inst` using [clj-time's `to-date` coercion](https://github.com/clj-time/clj-time#clj-timecoerce)
- `:level` - a `String`
- `:message` - a `String`
- `:category` - a `String`
- `:data` - a map with `String` keys and `String` values

## License

Copyright © 2016 Coda Hale

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
