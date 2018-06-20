{
  "info": {
    "name": "Clarify Get bundle metadata",
    "_postman_id": "87c0b0f5-832b-453a-88a9-afac14b1089e",
    "description": "Gets the metadata for a bundle.",
    "schema": "https://schema.getpostman.com/json/collection/v2.0.0/"
  },
  "item": [
    {
      "name": "Bundle",
      "item": [
        {
          "id": "2848752e-40a6-4c92-bd4c-422db581bf63",
          "name": "getV1BundlesBundleMetadata",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/metadata"
              ],
              "variable": [
                {
                  "id": "bundle_id",
                  "value": "{}",
                  "type": "string"
                }
              ]
            },
            "method": "GET",
            "body": {
              "mode": "raw"
            },
            "description": "Gets the metadata for a bundle."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "ad5b2583-e59a-49c7-9eb6-7ea18653c518"
            }
          ]
        },
        {
          "id": "840acaf8-936b-4fff-ad3c-f925490d371f",
          "name": "deleteV1BundlesBundleMetadata",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/metadata"
              ],
              "variable": [
                {
                  "id": "bundle_id",
                  "value": "{}",
                  "type": "string"
                }
              ]
            },
            "method": "DELETE",
            "body": {
              "mode": "raw"
            },
            "description": "Delete the metadata of a bundle and set data to {} (empty object.) This is functionally equivalent to an update metadata request with data set to {}.Successful response will be a HTTP code 204 with an empty body."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "7d8bee92-3905-4160-9530-31bc39b90a88"
            }
          ]
        }
      ]
    }
  ]
}