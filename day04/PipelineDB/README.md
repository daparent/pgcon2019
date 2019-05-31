# PiplineDB
* Streaming analytics - doing analytics on data in motion
* Two streaming databases:
	* PipelineDB
	* TimeScaleDB
* Data lake: data checks in but it can never check out
* Data stream: always moving, just passing through
* Data is infinite in the data stream, no bounds
* What can you do with it?
	* near real-time monitoring - HTTP response times grouped by page over past hour
	* near real-time stats - running totals of pageview counts, conversion rates, sales
	* A/B Testing: Get test results much faster, increases decision velocity
* analytics are done on the pipe and not the data warehouse
	* for example kafka
* streaming analytic databases tap into the stream
* 2 types of analytics:
	* continuous aggregation
		* start aggregation, time passes, do query, time passes do another query, now analyze the results
		* running totals
		* average
		* sums
	* sliding window
		* queries performed against sliding window as it exists at runtime
		* example: how many hits in a 10 second window 10 minutes ago
		* can see results over against a time dimension
		* pre-window data hasn't been looked at
		* post-window is no longer relavent (we don't care)
* pipeline emphasizes throughput over 100% accuracy
* a continuous view can join a stream of facts with a regular table of dimensions
* The rest of the talk is very much specific to PipelineDB
