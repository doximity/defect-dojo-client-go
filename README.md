# defect-dojo-client-go
DefectDojo API client in Go

This client is generated automatically from the DefectDojo OpenAPI 3.0 spec, using [github.com/deepmap/oapi-codegen](https://github.com/deepmap/oapi-codegen). So it has some quirks.

## Usage

Import the module:

```go
import (
	dd "github.com/doximity/defect-dojo-client-go"
	"github.com/deepmap/oapi-codegen/pkg/securityprovider"
)
```

Create a client:

```go
url := "https://demo.defectdojo.org"
token := os.Getenv("DOJO_APIKEY")

tokenProvider, err := securityprovider.NewSecurityProviderApiKey("header", "Authorization", fmt.Sprintf("Token %s", token))
if err != nil {
	panic(err)
}

client, err := dd.NewClientWithResponses(url, dd.WithRequestEditorFn(tokenProvider.Intercept))
```

Make a request (in this case create a product, i.e. `POST /products/`):

```go
apiResp, err := client.ProductsCreateWithResponse(ctx, dd.ProductsCreateJSONRequestBody{
	Name:        "My Product",
	Description: "A description",
	ProdType:    1,
})
```

Access fields from the response:

```go
if apiResp.StatusCode() == 201 {
	createdProductId := apiResp.JSON201.Id
	//...
}
```

## Development

To build the client, run:

```bash
$ oapi-codegen Defect-Dojo-API-v2.x.x.json > defectdojo.gen.go
```
