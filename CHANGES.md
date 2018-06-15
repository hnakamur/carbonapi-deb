Changes
=================================================

[Fix] - bugfix

**[Breaking]** - breaking change

[Feature] - new feature

[Improvement] - non-breaking improvement

[Code] - code quality related change that shouldn't make any significant difference for end-user

CHANGELOG
---------
**0.11.0**
 - **[Breaking][Fix] Allow to specify prefix for environment variables through `-envprefix` command line parameter. Default now is "CARBONAPI_" which might break some environments**
 - [Improvement] graphiteWeb function that implements force-fallback to graphtieWeb
 - [Improvement] graphiteWeb can query graphite-web 1.1.0+ for a list of supported functions and automatically do fallback to graphite in case user calls unimplemented function
 - [Improvement] New config switch to disable find request before render. This is helpful for graphite-clickhouse backends
 - [Fix] Fix typo in polyfit function description
 - [Fix] Skip metrics without pairs in reduceSeries (thx. to @errx)
 - [Fix] Fix config file parsing (thx. to @errx)
 - [Code] Move MakeResponse to expr/types and rename it to MakeMetricData. This is sometimes useful outside of tests. Thx. to @borovskyav
 - [Code] Add helper package that will ease doing per-function tests. See `expr/functions/absolute/function_test.go` for an example

**0.10.0.1**
 - [Fix] Autobuild scripts. Version bump to avoid retagging.

**0.10.0**
 - [Fix] `lineWidth` param is not working
 - [Fix] Support `&` in metric name
 - [Fix] default colors for png and svg are now defined as in graphite-web in 1.1.0, but add example of how to set them to new colors 
 - [Improvement] You can override default colors through config
 - [Improvement] It's now possible to specify 'template=' option to png or svg renders. It will use templates that's specified by 'graphTemplates' option in config. Template config format is not compatible with graphite-web though and uses either toml or yaml. Options naming is also different and follows URL override style.
 - [Code] Minor API changes in expr/png: MarshalPNGRequest and MarshalSVGRequest now requires to specify template.
 
**0.9.2**
 - [Improvement] "pipe" function chaining syntax. Same way as in https://github.com/graphite-project/graphite-web/pull/2042 (thx. to @lomik)
 
**0.9.1**
 - [Code] Refactor expr. Split it into pkg/parser. Simplify tests.
 - [Code] Refactor expr. Split functions into directories.
 - [Code] Simple docs about how to add a new function.
 - [Fix] All aggregating functions (e.x. summarize) now adds math.NaN() to begin and end of value list to make them same size.
 - [Fix] Experimental option `ExtrapolateExperiment` that allows aggregating functions to extrapolate data. Function is not tested properly yet and might be buggy, disabled by default
 - [Improvement] API to get list of functions (graphite-web 1.1 compatibility)
 - [Improvement] Add 'first' and 'last' as consolidateBy options

**0.9.0**
 - [Improvement] Merge carbonzipper into carbonapi. Please migrate to new 'upstreams' in the config file.
 - [Improvement] Make default cache timeout configurable.
 - [Improvement] Add divideSeriesLists (thx. to Oleg Matrokhin)
 - [Improvement] Add groupByNodes (thx to @errx)
 - [Improvement] Add cumulative(thx to Oleg Matrokhin)
 - [Improvement] Add diffSeriesLists and multiplySeriesLists functions (works the same way as divideSeriesLists vs divideSeries)
 - [Imprvoement] Add linearRegression function (thx. to Oleg Matrokhin)
 - [Improvement] Allow applyByNode to generate new targets
 - [Improvement] Allow diffSeries to substract series with different Steps
 - [Imprvoement] Make cache timeouts configurable
 - [Improvement] Make batch size configurable for sendGlobAsIs
 - [Improvement] Export GoVersion through expvars
 - [Improvement] Allow overriding config variables through ENV
 - [Improvement] Allow to use .toml in addition to .yaml as config file (determined by extension)
 - [Improvement] Add fallbackSeries (thx to @gksinghjsr)
 - [Improvement] Implement areaMode in cairo rendering (thx. to @ibuclaw)
 - [Improvement] Implement lineWidth, rightWidth, leftWidth (for cairo rendering) (thx. to @ibuclaw)
 - [Improvement] Add Unicode support for series names (thx. to @korservick)
 - [Improvement] Add multiplySeriesWithWildcards (thx. to @kamaev)
 - [Improvement] Add stddevSeries (thx. to @kamaev)
 - [Improvement] Add mapSeries (thx. to @kipwoker)
 - [Improvement] Add reduceSeries (thx. to @kipwoker)
 - [Improvement] Add delay() function (thx. to @kipwoker)
 - [Improvement] Update asPercent to graphite-web 1.1 (thx. to @kipwoker)
 - [Fix] Fix rounding errors when generating yLables for Cairo-based renders (thx. to @ibuclaw)
 - [Fix] Fix integer overflow when building for 32bit systems
 - [Fix] Don't set path in treejson if '.' not found in name (thx. to @ibuclaw)
 - [Fix] Fix derivative() if first arg is missing (thx. to Oleg Matrokhin)
 - [Fix] Fix calculation of yTop when using log base (thx. to @ibuclaw)
 - [Fix] Fix asPercent() to handle multiple series (thx. to @ibuclaw)
 - [Fix] Swap red and darkred colors (thx to @ibuclaw)
 - [Fix] Make asPercent graphite-compatible #213
 - [Fix] Fix summarize for cases when bucketSize < stepTime
 - [Fix] Fixed #90 - carbonapi shouldn't fail if one of the series doesn't exist
 - [Fix] Make PNG rendering looks like graphite-web (thx. to @lomik)
 - [Fix] Don't fail if argument is empty series (graphite-web compatibility, thx. to @gksinghjsr)
 - [Fix] Fix group() function #225 (thx. to @gksinghjsr)
 - [Code] Small refactor of PictureParams structs. Makes GetPictureParams and DefaultColorList public.

**0.8.0**
 - **[Breaking]** Most of the configuration options moved to a config file
 - **[Breaking]** Logging migrated to zap (structured logging). Log format changed significantly. Old command line options removed. Please consult carbonapi.example.yaml for a new config options and explanations
 - [Improvement] CarbonAPI now passes extra headers to keep track of requests
 - [Improvement] Allow '^' and '$' as name characters. Required for carbonsearch text-match pattern anchoring
 - [Improvement] Allow to bind to arbitrary IP:Port. (thx to @zdykstra, #194), Fixes #176
 - [Improvement] Extend legendValues to append multiple values
 - [Improvement] Add a switch to turn off propagating globbed requests to carbonzipper (was introduced in 0.7.0)
 - [Improvement] Add `{find,render}_cache_overhead_ns` metrics that shows cache query overhead in nanoseconds
 - [Improvement] Use dep as a vendoring tool
 - [Improvement] Add a Makefile that will hide some magic from user.
 - [Improvement] Send glob heruistic -- if request with globs is relatively small (<100 metrics) carbonapi will send it as is.
 - [Improvement] divideSeries support seriesList in dividents (#203, thx @cldellow)
 - [Improvement] added movingMin/movingMax/movingSum (#206, thx @cldellow)
 - [Feature] Implement following functions from graphite-web: linewidth, areaBetween, applyByNode
 - [Feature] Add fft, ifft, LowPass functions. See COMPATIBILITY.md for more information
 - [Feature] Publish current configuration as an expvar (GET /debug/vars)
 - [Feature] Make prefix configurable (thx to @StephenPCG, #189)
 - [Fix] Fix issue with missing labels on graphs > 14d
 - [Fix] Allow expressions to use 'total' instead of 'sum'. Required for graphite compatibility
 - **[Fix]** Fix incompatibility between carbonapi and older versions of carbonzipper/carbonserver/go-carbon (protobuf2-only)

Notes on upgrading:

Even though there are several changes that's marked as breaking, it only breaks local config parsing and changes logging format.

**This release fixes the incompatibility introduced in 0.6.0 and can work with both new and old versions of carbonserver, carbonzipper, go-carbon**

**0.7.0**
 - [Improvement] Render requests with globs now sent as-is.
 - [Improvement] Added ProxyHeaders handler to get real client's ip address from X-Real-IP or X-Forwarded-For headers.
 - [Improvement] Highest\* and Lowest\* now assumes '1' if no second argument was passed
 - [Feature] Implement legendValue expression (see http://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.legendValue for more information)

Notes on upgrading to 0.7.0:

It's highly recommended, if you use carbonzipper to also upgrade it to version 0.62

**0.6.0**
 - **[Breaking]** Migrate to protobuf3

Notes on upgrading to 0.6.0:

You need to upgrade carbonzipper or go-carbon to the version that supports protobuf3

**<=0.5.0**
There is no dedicated changelog for older versions of carbonapi. Please see commit log for more information about what changed for each commit.
