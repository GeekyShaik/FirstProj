{
  "openapi": "3.0.1",
  "info": {
    "title": "V2IrisTemplateMetadata",
    "description": "This is the iris template metadata api that will be used by both iris internal components and external clients who consumes iris public apis",
    "contact": {
      "name": "Librarian",
      "email": "usaa_moc_librarians_auth_sa@usaa.com"
    },
    "version": "v2.0.5",
    "x-usaa-application": "Iris Experience Layer for Migration",
    "x-usaa-category": "Enterprise"
  },
  "servers": [
    {
      "url": "https://rintapi-ent.usaa.com/v2/enterprise/communications/iris/metadata"
    }
  ],
  "tags": [
    {
      "name": "/runtime-metadata",
      "description": "call v3 Core API to GET RUNTIME_MDATA_TXT by mdata-obj-type-cd"
    }
  ],
  "paths": {
    "/runtime-metadata": {
      "get": {
        "tags": [
          "/runtime-metadata"
        ],
        "operationId": "getTemplateRuntimeMetadata",
        "parameters": [
          {
            "name": "metadataRequest",
            "in": "query",
            "required": true,
            "schema": {
              "$ref": "#/components/schemas/MetadataRequest"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/TemplateRuntimeMetadataDTO"
                }
              }
            }
          },
          "400": {
            "description": "Bad Request"
          },
          "401": {
            "description": "Unauthorized"
          },
          "403": {
            "description": "Forbidden"
          },
          "500": {
            "description": "Internal Server Error"
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "MetadataRequest": {
        "required": [
          "mdataObjEffDate",
          "mdataObjName",
          "mdataObjTypeCode"
        ],
        "type": "object",
        "properties": {
          "mdataObjName": {
            "maxLength": 50,
            "minLength": 1,
            "type": "string"
          },
          "mdataObjTypeCode": {
            "maxLength": 20,
            "minLength": 0,
            "type": "string"
          },
          "mdataObjEffDate": {
            "type": "string",
            "format": "date"
          }
        }
      },
      "JsonNode": {
        "type": "object"
      },
      "TemplateRuntimeMetadataDTO": {
        "required": [
          "creaPrtyId",
          "creaTimestamp",
          "envrName",
          "mdataObjEffDate",
          "mdataObjName",
          "mdataObjTypeCode",
          "mdataObjVerNumber",
          "mdataObjVerStatCode",
          "refObjVerNumber",
          "runtimeMdataText"
        ],
        "type": "object",
        "properties": {
          "mdataObjName": {
            "type": "string"
          },
          "mdataObjTypeCode": {
            "type": "string"
          },
          "mdataObjEffDate": {
            "type": "string",
            "format": "date"
          },
          "mdataObjVerNumber": {
            "type": "integer",
            "format": "int32"
          },
          "envrName": {
            "type": "string"
          },
          "refObjVerNumber": {
            "type": "number"
          },
          "mdataObjVerStatCode": {
            "type": "string"
          },
          "runtimeMdataText": {
            "$ref": "#/components/schemas/JsonNode"
          },
          "creaTimestamp": {
            "type": "string",
            "format": "date-time"
          },
          "creaPrtyId": {
            "type": "string"
          },
          "updtTimestamp": {
            "type": "string",
            "format": "date-time"
          },
          "updtPrtyId": {
            "type": "string"
          }
        }
      }
    },
    "securitySchemes": {
      "API Key": {
        "type": "apiKey",
        "name": "api-key",
        "in": "header"
      },
      "oauth2": {
        "type": "oauth2",
        "flows": {
          "implicit": {
            "scopes": {
              "usaa.ent.authoring.public-apis.read": "This endpoint will allow requests to retrieve runtime metadata from APP_IRIS"
            },
            "authorizationUrl": "https://example.com"
          }
        }
      }
    }
  }
}
