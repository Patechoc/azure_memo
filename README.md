# azure_memo
Simple useful commands to run against Azure 

- [Reference list of Azure services](https://docs.microsoft.com/en-us/cli/azure/azure-services-the-azure-cli-can-manage)

## Installation tricks

- instruct `pip` to use the system certificate store instead of the "incomplete" bundled cacert that comes with it by installing `pip-system-certs`

`pip install pip-system-certs`

## Login to Azure & switch subscription

### Log in

`az login`

### List your subscriptions 

`az account list --output table`

### Switch subscription

`az account set --subscription <name or id>`

## Management and Governance

### (Resource) Groups

#### List resource groups

`az group list -o table`

## Compute

### Function Apps

- List all FunctionApps: `az functionapp list -o table`

## DevOps

### Monitor

#### Log Analytics

[reference](https://docs.microsoft.com/en-us/cli/azure/monitor/log-analytics)

1. Find the Log Analytics Workspace ID you want to query
2. Run a specific query:

```shell
az monitor log-analytics query -w 7893d280-08de-4763-a795-b36f4432fddd --analytics-query "AppTraces | where TimeGenerated > ago(10m) "
```

#### Application Insights

1. Find the (Application Insights) app attached to the resource you are interested in. 
2. Click the `JSONview` and find the `AppID` of the app (something like `14f0953-ad40-42c5-b29d-debc616b215fd`)
3. Search for the **metric** you are interested in among the huge list provided by `get-metadata`

```shell
(.venv) λ az monitor app-insights metrics get-metadata --app 14215fd3-ad40-42c5-b29d-def095bc616b | grep "requests"
    "performanceCounters/requestsInQueue": {
      "displayName": "ASP.NET requests in application queue",
    "performanceCounters/requestsPerSecond": {
    "requests/count": {
      "displayName": "Server requests",
    "requests/duration": {
    "requests/failed": {
      "displayName": "Failed requests",
```

4. show the metric's data on a given time interval ([very badly documented by Microsoft](https://docs.microsoft.com/en-us/cli/azure/monitor/app-insights/metrics?view=azure-cli-latest#az-monitor-app-insights-metrics-show))

```shell
(.venv) λ az monitor app-insights metrics show --app 14215fd3-ad40-42c5-b29d-def095bc616b --metric requests/count --start-time 2022-09-12 --end-time 2022-09-13
{
  "value": {
    "end": "2022-09-12T22:00:00+00:00",
    "interval": null,
    "requests/count": {
      "sum": 280
    },
    "segments": null,
    "start": "2022-09-11T22:00:00+00:00"
  }
}
```

```shell
(.venv) λ az monitor app-insights metrics show --app 14215fd3-ad40-42c5-b29d-def095bc616b --metric requests/count --start-time 2022-09-12 01:00:00 --end-time 2022-09-12 02:00:00
{
  "value": {
    "end": "2022-09-12T00:00:00+00:00",
    "interval": null,
    "requests/count": {
      "sum": 12
    },
    "segments": null,
    "start": "2022-09-11T23:00:00+00:00"
  }
}
```

### Pipelines
