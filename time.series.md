# Time Series Concepts

In this article we will go through common concepts that we work with in the Timeseries Insights API and try to provide an intuitive explanation on what they represent.

## Event

An event is a data point and the raw input that the Timeseries Insights API works with. Conceptually it represents either an action being carried out by some agent (e.g. a transaction by a client or the publishing of a news article) or an observation (e.g. the readings of a temperature sensor, or CPU usage on a machine).

An event contains:

- A set of values across different [dimensions](https://cloud.google.com/timeseries-insights/docs/concept#dimension), representing properties which describe the event, such as labels or numerical measurements.
- A timestamp representing the time when the event occurred. This timestamp will be used when aggregating events to form a [time series](https://cloud.google.com/timeseries-insights/docs/concept#time_series).
- A [group](https://cloud.google.com/timeseries-insights/docs/concept#group) id.

**Note:** The time when the event occurred (and thus, the timestamp field) can be viewed conceptually as simply another dimension, but, given that it is always required and most of the operations depend on it, it has its own dedicated field.

**Note:** In some cases, an event may include some measurements over a short interval instead of just one point in time. For example, if in our [dataset](https://cloud.google.com/timeseries-insights/docs/concept#dataset) we store production monitoring data (such as cpu usage, error rates, etc), then one event may be a measurement over a one minute interval. In these scenarios it is important to be consistent with the length of these measurement intervals (e.g. always be one minute in this example).

## Dimension

A dimension represents a property type for the events in a dataset and the domain of values it can take. A dimension can be:

- Categorical. An event property on this dimension can hold one of a limited/finite values, usually strings. Examples include: the country or publisher name in a dataset with news articles, the machine name in a dataset with production monitoring data.
- Numerical. A measurement or a general numerical property for an event. Examples: number of page views for news articles, CPU usage or number of errors for production monitoring data.

**Note:** In the API definition, a `EventDimension` with bool/string values is a categorical dimension, while dimensions with long/double values are numerical.

## Dataset

A dataset is a collection of events with a unique name within a project. Queries are performed within the same dataset.

## Group

Events can be grouped together by specifying the same group id (see `Event.groupId`). The group is similar to a "session" of internet activities.

Most commonly, each [Event](https://cloud.google.com/timeseries-insights/docs/reference/rest/v1/projects.locations.datasets/appendEvents#event) record is given a unique group ID. Use cases of group ID also include but are not limited to:

- An event identifier for the same event (with the same or similar timestamps) from multiple [Event](https://cloud.google.com/timeseries-insights/docs/reference/rest/v1/projects.locations.datasets/appendEvents#event) records, especially when different properties of the same event come from different sources and not merged before entering the system.  For example, several sensors monitoring the same device can each produce a separate event record.
- A session identifier for a collection of related events (typically with timestamps within a short period).  An example is activities from a web browsing session. Another example is log entries from a taxi ride.
- A user account identifier, so all [Event](https://cloud.google.com/timeseries-insights/docs/reference/rest/v1/projects.locations.datasets/appendEvents#event) records with the same group ID belongs to the same user.

The purpose of the group is to compute correlations among (dimensions of) events from the same group. For example, if your dataset holds monitoring data (such as CPU, RAM, etc), then a group could hold all the monitoring data from one process. That would eventually allow us to detect that an increase in CPU is correlated with another event, such as a binary version update at a previous moment in time.

If unsure, or if not interested in computing these types of correlations, then each event should have a globally unique group id.  Omitting `groupId` has a similar effect and an internal `groupId` is generated based on the contents and the timestamp.

## Slice

A slice is the subset of all events from a dataset that have some given values across some **[dimensions](https://cloud.google.com/timeseries-insights/docs/concept#dimension)**.  For a categorical dimension, the given value is a single fixed value; for a numerical dimension, the given value is a range.

For example, let's consider we have a dataset with the sales from an international retailer and each event is a sale that has these categorical dimensions: the country where the sale occurred, the name of the product, the name of the company that made the product. Examples of slices in this case are: all the sales for a given product, all the sales from a given country for all the products made by a given company.

## Time series

The time series we work with are of discrete time, composed of points at equal time intervals. The length of the time intervals between consecutive time series points is called the **granularity** of the time series.

A time series is computed by:

- For a given slice, collect all events in the [`detectionTime - TimeseriesParams.forecastHistory`, `detectionTime + granularity`] time interval.
- Group these events, based on their timestamp and granularity. An event **E** is assigned to a point that starts at time **T** if `E.eventTime` is in the [`T`, `T + granularity`] time interval.
- Aggregate, for every point in the time series, the events based on the specified numerical dimension as **metric** (`TimeseriesParams.metric`), which represents the value for those points.  The aggregation can be done by counting (if no `metric` is specified, typically if all dimensions of the event are categorical), summing or averaging (if `metric` is specified).

**Note:** The `forecastHistory` is a time period **before** `detectionTime`.

## Time series point

Each time series point has an associated **time** and **value**.

The **time** is actually an interval of the length `granularity` with `time` as the starting time.

If **metric** (`TimeseriesParams.metric`) is specified, it needs to be a numerical dimension. The `value` of the point is aggregated from the dimension values in the `metric` dimension of all events within the time internal, using `TimeseriesParams.metricAggregationMethod`.

If no metric is specified, the `value` of the point is the number of events within the time interval.

## Forecasting

The process of predicting future values for a given time series.  The forecasting uses the beginning part of the time series as training data to build a model.

## Horizon

We will forecast the values of a time series starting from the [detection time](https://cloud.google.com/timeseries-insights/docs/concept#detection_time_and_detection_point) up to the time horizon (given by the `ForecastParams.horizonTime` field).

Intuitively, this field tells us how much in the future we should forecast. While we are mostly interested in the value of the detection point when classifying a slice as an anomaly, we allow extra points to be forecasted as it may provide useful information for the user.

**Note:** The `horizonTime` is a time **after** `detectionTime`.

## Detection time and detection point

The detection time (specified by `QueryDataSetRequest.detectionTime`) is the point in time that we are analyzing for any potential [anomalies](https://cloud.google.com/timeseries-insights/docs/concept#anomaly).

The detection point is the time series point at the detection time.

## Expected deviation

Depending on the stability and the predictability of a time series we can be less or more confident in our forecast. The confidence level is reflected in the `EvaluatedSlice.expectedDeviation` field, which specifies what is an acceptable absolute deviation from the value we forecasted for the detection time.

## Anomaly

A [slice](https://cloud.google.com/timeseries-insights/docs/concept#slice) can be considered an anomaly if the deviation between its forecasted and actual values during the detection time is higher than what [we expect it to be](https://cloud.google.com/timeseries-insights/docs/concept#expected_deviation).

We call how far the actual deviation is from the expected deviation the **anomaly score**:

***anomalyScore = (detectionPointActual - detectionPointForecast) / expectedDeviation\***

In general, scores lower than 1.0 reflect variations that are common considering the history of the slice, while scores higher than 1.0 should require attention, with higher scores representing more severe anomalies.

## Noise threshold

The [anomaly score](https://cloud.google.com/timeseries-insights/docs/concept#anomaly) as previously defined shows how statistically significant a deviation is from the normal. However, in many cases, this deviation might not be important because the change in absolute value might not be of interest.

For example, if a time series has all its values uniformly distributed in the *(9.9, 10.1)* range, then the `expectedDeviation` will be ~0.1. If the `detectionPointForecast` is *10.0* and the `detectionPointActual` is *10.3*, then the `anomalyScore` will be *3.0*.

If changes that are greater in absolute value are more important to you, the **noise threshold** offers a way to penalize slices with changes lower than that threshold by weighing down the anomaly score:

*anomalyScore = (detectionPointActual - detectionPointForecast) / (expectedDeviation + **noiseThreshold**)*

## What's next

- Follow [Setup for Full Access](https://cloud.google.com/timeseries-insights/docs/getting-started) to create your own project
- A more detailed [Tutorial](https://cloud.google.com/timeseries-insights/docs/tutorial)
- Learn more about the [REST API](https://cloud.google.com/timeseries-insights/docs/reference/rest/v1/projects.locations.datasets)

