

# libDatabox
`import "./"`

* [Overview](#pkg-overview)
* [Index](#pkg-index)
* [Subdirectories](#pkg-subdirectories)

## <a name="pkg-overview">Overview</a>
A golang library for interfacing with Databox APIs.

Install using go get github.com/tsh2/lib-go-databox

Examples can be found in the samples directory




## <a name="pkg-index">Index</a>
* [func ExportLongpoll(destination string, payload string) (string, error)](#ExportLongpoll)
* [func GetHttpsCredentials() string](#GetHttpsCredentials)
* [type BinaryKeyValue_0_2_0](#BinaryKeyValue_0_2_0)
  * [func NewBinaryKeyValueClient(reqEndpoint string, enableLogging bool) (BinaryKeyValue_0_2_0, error)](#NewBinaryKeyValueClient)
* [type DataSourceMetadata](#DataSourceMetadata)
  * [func HypercatToDataSourceMetadata(hypercatDataSourceDescription string) (DataSourceMetadata, string, error)](#HypercatToDataSourceMetadata)
* [type JSONKeyValue_0_2_0](#JSONKeyValue_0_2_0)
  * [func NewJSONKeyValueClient(reqEndpoint string, enableLogging bool) (JSONKeyValue_0_2_0, error)](#NewJSONKeyValueClient)
* [type JSONTimeSeries_0_2_0](#JSONTimeSeries_0_2_0)
  * [func NewJSONTimeSeriesClient(reqEndpoint string, enableLogging bool) (JSONTimeSeries_0_2_0, error)](#NewJSONTimeSeriesClient)
* [type TextKeyValue_0_2_0](#TextKeyValue_0_2_0)
  * [func NewTextKeyValueClient(reqEndpoint string, enableLogging bool) (TextKeyValue_0_2_0, error)](#NewTextKeyValueClient)


#### <a name="pkg-files">Package files</a>
[core-store-kv-bin.go](/src/target/core-store-kv-bin.go) [core-store-kv-json.go](/src/target/core-store-kv-json.go) [core-store-kv-text.go](/src/target/core-store-kv-text.go) [core-store-ts-json.go](/src/target/core-store-ts-json.go) [export.go](/src/target/export.go) [utils.go](/src/target/utils.go) 





## <a name="ExportLongpoll">func</a> [ExportLongpoll](/src/target/export.go?s=339:410#L5)
``` go
func ExportLongpoll(destination string, payload string) (string, error)
```
ExportLongpoll exports data to external service (payload must be an escaped json string)
permissions must be requested in the app manifest (drivers dont need to use the export service)



## <a name="GetHttpsCredentials">func</a> [GetHttpsCredentials](/src/target/utils.go?s=2426:2459#L93)
``` go
func GetHttpsCredentials() string
```
GetHttpsCredentials Returns a string containing the HTTPS credentials to pass to https server when offering an https server.
These are read form /run/secrets/DATABOX.pem and are generated by the container-manger at run time.




## <a name="BinaryKeyValue_0_2_0">type</a> [BinaryKeyValue_0_2_0](/src/target/core-store-kv-bin.go?s=109:494#L1)
``` go
type BinaryKeyValue_0_2_0 interface {
    // Write text value
    Write(dataSourceID string, payload []byte) error
    // Read text values.
    Read(dataSourceID string) ([]byte, error)
    // Read JSON values.
    // Get notifications of updated values
    Observe(dataSourceID string) (<-chan []byte, error)
    // Get notifications of updated values
    RegisterDatasource(metadata DataSourceMetadata) error
}
```






### <a name="NewBinaryKeyValueClient">func</a> [NewBinaryKeyValueClient](/src/target/core-store-kv-bin.go?s=861:959#L21)
``` go
func NewBinaryKeyValueClient(reqEndpoint string, enableLogging bool) (BinaryKeyValue_0_2_0, error)
```
NewBinaryKeyValueClient returns a new NewBinaryKeyValueClient to enable reading and writing of binary data key value to the store
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.





## <a name="DataSourceMetadata">type</a> [DataSourceMetadata](/src/target/utils.go?s=4560:4799#L186)
``` go
type DataSourceMetadata struct {
    Description    string
    ContentType    string
    Vendor         string
    DataSourceType string
    DataSourceID   string
    StoreType      string
    IsActuator     bool
    Unit           string
    Location       string
}
```






### <a name="HypercatToDataSourceMetadata">func</a> [HypercatToDataSourceMetadata](/src/target/utils.go?s=7043:7150#L254)
``` go
func HypercatToDataSourceMetadata(hypercatDataSourceDescription string) (DataSourceMetadata, string, error)
```
HypercatToDataSourceMetadata is a helper function to convert the hypercat description of a datasource to a DataSourceMetadata instance
Also returns the store url for this data source.





## <a name="JSONKeyValue_0_2_0">type</a> [JSONKeyValue_0_2_0](/src/target/core-store-kv-json.go?s=109:723#L1)
``` go
type JSONKeyValue_0_2_0 interface {
    // Write JSON value
    Write(dataSourceID string, payload []byte) error
    // Read JSON values. Returns a []bytes containing a JSON string.
    Read(dataSourceID string) ([]byte, error)
    // Get notifications of updated values Returns a channel that receives []bytes containing a JSON string when a new value is added.
    Observe(dataSourceID string) (<-chan []byte, error)
    // RegisterDatasource make a new data source for available to the rest of datbox. This can only be used on stores that you have requested in your manifest.
    RegisterDatasource(metadata DataSourceMetadata) error
}
```






### <a name="NewJSONKeyValueClient">func</a> [NewJSONKeyValueClient](/src/target/core-store-kv-json.go?s=1082:1176#L20)
``` go
func NewJSONKeyValueClient(reqEndpoint string, enableLogging bool) (JSONKeyValue_0_2_0, error)
```
NewJSONKeyValueClient returns a new NewJSONKeyValueClient to enable reading and writing of JSON data key value to the store
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.





## <a name="JSONTimeSeries_0_2_0">type</a> [JSONTimeSeries_0_2_0](/src/target/core-store-ts-json.go?s=107:2249#L1)
``` go
type JSONTimeSeries_0_2_0 interface {
    // Write  will be timestamped with write time in ms since the unix epoch by the store
    Write(dataSourceID string, payload []byte) error
    // WriteAt will be timestamped with timestamp provided in ms since the unix epoch
    WriteAt(dataSourceID string, timestamp int64, payload []byte) error
    // Read the latest value.
    // return data is a JSON object of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Latest(dataSourceID string) ([]byte, error)
    // Read the earliest value.
    // return data is a JSON object of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Earliest(dataSourceID string) ([]byte, error)
    // Read the last N values.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    LastN(dataSourceID string, n int) ([]byte, error)
    // Read the first N values.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    FirstN(dataSourceID string, n int) ([]byte, error)
    // Read values written after the provided timestamp in in ms since the unix epoch.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Since(dataSourceID string, sinceTimeStamp int64) ([]byte, error)
    // Read values written between the start timestamp and end timestamp in in ms since the unix epoch.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Range(dataSourceID string, formTimeStamp int64, toTimeStamp int64) ([]byte, error)
    // Get notifications when a new value is written
    // the returned chan receives valuse of the form {"timestamp":213123123,"data":[data-written-by-driver]}
    Observe(dataSourceID string) (<-chan []byte, error)
    // registerDatasource is used by apps and drivers to register data sources in stores they own.
    RegisterDatasource(metadata DataSourceMetadata) error
    // GetDatasourceCatalogue is used by drivers to get a list of registered data sources in stores they own.
    GetDatasourceCatalogue() ([]byte, error)
}
```






### <a name="NewJSONTimeSeriesClient">func</a> [NewJSONTimeSeriesClient](/src/target/core-store-ts-json.go?s=2585:2683#L41)
``` go
func NewJSONTimeSeriesClient(reqEndpoint string, enableLogging bool) (JSONTimeSeries_0_2_0, error)
```
NewJSONTimeSeriesClient returns a new jSONTimeSeriesClient to enable interaction with a time series data store in JSON format
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.





## <a name="TextKeyValue_0_2_0">type</a> [TextKeyValue_0_2_0](/src/target/core-store-kv-text.go?s=109:736#L1)
``` go
type TextKeyValue_0_2_0 interface {
    // Write text value
    Write(dataSourceID string, payload string) error
    // Read text values. Returns a string containing the text written to the key.
    Read(dataSourceID string) (string, error)
    // Get notifications of updated values Returns a channel that receives strings containing a text string when a new value is added.
    Observe(dataSourceID string) (<-chan string, error)
    // RegisterDatasource make a new data source for available to the rest of datbox. This can only be used on stores that you have requested in your manifest.
    RegisterDatasource(metadata DataSourceMetadata) error
}
```






### <a name="NewTextKeyValueClient">func</a> [NewTextKeyValueClient](/src/target/core-store-kv-text.go?s=1094:1188#L20)
``` go
func NewTextKeyValueClient(reqEndpoint string, enableLogging bool) (TextKeyValue_0_2_0, error)
```
NewTextKeyValueClient returns a new TextKeyValue_0_2_0 to enable reading and writing of string data key value to the store
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.









- - -
Generated by [godoc2md](http://godoc.org/github.com/davecheney/godoc2md)
