{
  "info": {
    "name": "Clarify Request an insight to be run",
    "_postman_id": "0ec50c07-de04-4b4a-a3a6-37e1197c1cd2",
    "description": "Request an insight to be run on a bundle. Note that most insights are set to automatically run on all bundles so you commonly won&apos;t need to call this endpoint except to request transcripts. To configure which insights are automatically run for an app, visit the Clarify Developer Portal. Insights that are not configured to autorun can be requested to run on an individual bundle using this endpoint. The following insights can be requested:transcript_r9 - High-accuracy transcript of the speech in audio media.Transcripts will produced on the mixed audio of all tracks in the bundle and are charged per minute (rounded up for partial minutes), based on the duration of the longest track. If the request has already been made, this method has no effect other than to return the existing insight.Transcripts will typically take about 48 hours. When the transcript is ready, an InsightNotification webhook will be POSTed to the bundle notify_url.For more information see Human Transcripts Quick Start.captions_r9 - High-accuracy captions of the speech in video media.Captions will be generated on the first track in the bundle. and are charged per minute (rounded up for partial minutes), based on the duration of the media.  See the pricing page. If the request has already been made, this method has no effect other than to return the existing insight.Captions will typically take about 72 hours. When the captions are ready, an InsightNotification webhook will be POSTed to the bundle notify_url.For more information see Captions Quick Start.spoken_keywords - Spoken words of interest found in audio media. Note: Normally spoken_keywords is set to autorun so you do not need to run it explicitly.spoken_topics - Topics spoken about in the audio media.",
    "schema": "https://schema.getpostman.com/json/collection/v2.0.0/"
  },
  "item": [
    {
      "name": "List",
      "item": [
        {
          "id": "6ede47a3-a727-4600-8ae0-f190b79cf7c0",
          "name": "getV1Bundles",
          "request": {
            "url": "http://api.clarify.io/v1/bundles?embed=%7B%7D&iterator=%7B%7D&limit=%7B%7D",
            "method": "GET",
            "body": {
              "mode": "raw"
            },
            "description": "Gets the list of bundles. Links to each item are in the _links with link relation items.After getting the initial list, use the first, last, next, prev link relations to get more bundles in the list. Note that next will not be available at the end of the list and prev will not be available at the start of the list. If the results are exactly one page neither prev nor next will be available.The embed parameter specifies link relations to embed in the results. The models for the specified link relations will be in an array in the embedded object with the link relation as the key. For example, if you do embed=items, _embedded will contain a property items whose value is the array of bundle models. For link relations that are curies (ex. \"clarify:metadata\"), you may simply use the base name (ex. \"metadata\")."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "77f783d1-0e5d-485e-ac91-7008733949b6"
            }
          ]
        }
      ]
    },
    {
      "name": "Bundle",
      "item": [
        {
          "id": "5dbe6e1b-5b46-428d-8ba9-67511ece4cfb",
          "name": "postV1Bundles",
          "request": {
            "url": "http://api.clarify.io/v1/bundles",
            "method": "POST",
            "body": {
              "mode": "urlencoded",
              "urlencoded": [
                {
                  "key": "audio_channel",
                  "value": "{}",
                  "disabled": false,
                  "description": "The audio channel to use for the track (  | left | right )"
                },
                {
                  "key": "audio_language",
                  "value": "{}",
                  "disabled": false,
                  "description": "Language of the audio in the track, specified with an RFC5646 code"
                },
                {
                  "key": "external_id",
                  "value": "{}",
                  "disabled": false,
                  "description": "A string that can refer to an item in an external system"
                },
                {
                  "key": "label",
                  "value": "{}",
                  "disabled": false,
                  "description": "Label for the track (if media_url is specified"
                },
                {
                  "key": "media_url",
                  "value": "{}",
                  "disabled": false,
                  "description": "URL of a media (audio or video) file for this bundle"
                },
                {
                  "key": "metadata",
                  "value": "{}",
                  "disabled": false,
                  "description": "User-defined JSON data associated with the bundle"
                },
                {
                  "key": "name",
                  "value": "{}",
                  "disabled": false,
                  "description": "Name of the bundle"
                },
                {
                  "key": "notify_url",
                  "value": "{}",
                  "disabled": false,
                  "description": "URL for notifications on this bundle"
                },
                {
                  "key": "parts_pending",
                  "value": "{}",
                  "disabled": false,
                  "description": "Set to true if more media parts will be added to the track"
                },
                {
                  "key": "start_time",
                  "value": "{}",
                  "disabled": false,
                  "description": "Time offset in seconds that the media starts relative to the bundle"
                }
              ]
            },
            "description": "Create a new bundle with the specified name, media url, and optional JSON metadata.name can be any string you wish to associate with the bundle.media_url must be a publicly accessible url to a media file. It will be fetched asynchronously after the REST call returns. The audio can be mono or stereo.audio_channel is used to specify audio channels if the media is a stereo file. A value of left or right signifies that only the specified channel will be used. If no value or an empty string is specified for audio_channel, all channels will be used in a single track. If your stereo channels were recorded separately with each channel containing distinct content (for example if 2 legs of a phone call were recorded separately and combined into a single stereo file), for best speech recognition, create two tracks, with audio_channel set to left and right in each track respectively. If your stereo file is simply a recording made with a stereo microphone, audio_channel should be set to an empty string (or not be specified.) If you have audio channels as separate media files, after creating the bundle with one media_url, POST another media_url to /bundles/{bundle_id}/tracks.audio_language can be used to specify the language of the audio media. This is an optional parameter and if not specified or an empty string, the language of the track will be automatically detected. If specified, it must be a language code as described in RFC5646 (see http://tools.ietf.org/html/rfc5646). Supported languages: en-US, en-UK, es, fr.label is a short name for the track.metadata is a single-level JSON object of your own definition, containing key-values that can be searched and filtered on. Metadata can be used to hold text such as names, titles, descriptions and values for segregating bundles, for example by user, topic, folder name etc. The keys (property names) can be up to 64 characters and must contain only alphanumeric characters and underscore (but not start with underscore) and must not be a reserved name. Reserved names are &quot;true&quot;, &quot;false&quot;, and &quot;null&quot;. Values can be strings, numbers, boolean true/false, date-times represented as a string in ISO 8601 format (ex. &quot;2014-02-25T14:23:45.000Z&quot;), or an array of these primitive types. Strings can be up to 2000 characters and strings in arrays can be up to 128 characters each. Nested objects are not allowed. Metadata can contain up to 50 key-value pairs up to a total JSON size of 4000 characters.start_time a time in seconds that the media starts, relative to start time of the bundle. This allows you to specify sequential parts of media. If not specified, the default is 0.parts_pending a boolean flag specifying if more media parts will subsequently be added to the track. If true, a subsequent API call must be made to signify that the track is complete. If not specified, the default is false.external_id is an optional parameter that can be used to logically link a bundle to an item in an external system. The external_id can be whatever you use to identify items in your own database.notify_url is a webhook. It must be a publicly accessible url (http or https) on your server to which notifications for the bundle will be POSTed. There are three types of notifications: Track Notifications, Insight Notifications and Bundle Notifications. For more information on the content of notifications and when they are sent, see the notification docs page.If a track was created along with the budle, the link relation clarify:track will be included with a link to the new track."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "adf8b549-46b2-4287-8356-f9f750cdcc7a"
            }
          ]
        },
        {
          "id": "6f36f93c-15df-4388-9ce0-74339050816f",
          "name": "getV1BundlesBundle",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id"
              ],
              "query": [
                {
                  "key": "embed",
                  "value": "%7B%7D",
                  "disabled": false
                }
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
            "description": "Get a bundle that has previously been created."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "c9541c38-02c0-4d43-959e-a6db93a90dff"
            }
          ]
        },
        {
          "id": "31f854e5-cf11-4416-893c-fdfa548c6b96",
          "name": "putV1BundlesBundle",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id"
              ],
              "variable": [
                {
                  "id": "bundle_id",
                  "value": "{}",
                  "type": "string"
                }
              ]
            },
            "method": "PUT",
            "body": {
              "mode": "urlencoded",
              "urlencoded": [
                {
                  "key": "external_id",
                  "value": "{}",
                  "disabled": false,
                  "description": "A string that can refer to an item in an external system"
                },
                {
                  "key": "name",
                  "value": "{}",
                  "disabled": false,
                  "description": "Name of the bundle"
                },
                {
                  "key": "notify_url",
                  "value": "{}",
                  "disabled": false,
                  "description": "URL for notifications on this bundle"
                },
                {
                  "key": "version",
                  "value": "{}",
                  "disabled": false,
                  "description": "Object version"
                }
              ]
            },
            "description": "Update a bundle. To update the tracks, media, or metadata of a bundle, use the tracks and metadata endpoints.name can be any string you wish to associate with the bundle.external_id is an optional parameter that can be used to logically link a bundle to an item in an external system. The external_id can be whatever you use to identify items in your own database.notify_url is a webhook. It must be a publicly accessible url (http or https) on your server to which notifications for the bundle will be POSTed. There are three types of notifications: Track Notifications, Insight Notifications and Bundle Notifications. For more information on the content of notifications and when they are sent, see the notification docs page.If version is specified, the bundle will only be updated if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the bundle will always be updated."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "6c2cb842-8075-46ef-9b4c-7c24400cebef"
            }
          ]
        },
        {
          "id": "8ca88583-b34a-47cf-864b-2370af7082df",
          "name": "deleteV1BundlesBundle",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id"
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
            "description": "Delete a bundle and its related metadata and tracks. This will only delete media stored on Clarify systems and not delete the source media on remote systems.Successful response will be a HTTP code 204 with an empty body."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "a130939d-30ef-49d0-a2c8-bf9aba53ce1a"
            }
          ]
        },
        {
          "id": "11e2fb99-8cc5-4b70-9ec5-419d150a5322",
          "name": "getV1BundlesBundleInsights",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/insights"
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
            "description": "Gets the insights for a bundle.URLs of the available insights for the bundle are in the _links object, with the link relations (keys) of the format insight:insight_name.Documentation on the insights available and the data returned can be found at http://docs.clarify.io/insights/"
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "297911da-0022-4623-a73b-be785e9f6f8c"
            }
          ]
        }
      ]
    },
    {
      "name": "Request",
      "item": [
        {
          "id": "f8f6b9f9-b537-49c2-b1e0-4c7f58ae0e7e",
          "name": "postV1BundlesBundleInsights",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/insights"
              ],
              "variable": [
                {
                  "id": "bundle_id",
                  "value": "{}",
                  "type": "string"
                }
              ]
            },
            "method": "POST",
            "body": {
              "mode": "urlencoded",
              "urlencoded": [
                {
                  "key": "insight",
                  "value": "{}",
                  "disabled": false,
                  "description": "name of the insight: transcript_r9, captions_r9, spoken_keywords, spoken_topics, spoken_words"
                }
              ]
            },
            "description": "Request an insight to be run on a bundle. Note that most insights are set to automatically run on all bundles so you commonly won&apos;t need to call this endpoint except to request transcripts. To configure which insights are automatically run for an app, visit the Clarify Developer Portal. Insights that are not configured to autorun can be requested to run on an individual bundle using this endpoint. The following insights can be requested:transcript_r9 - High-accuracy transcript of the speech in audio media.Transcripts will produced on the mixed audio of all tracks in the bundle and are charged per minute (rounded up for partial minutes), based on the duration of the longest track. If the request has already been made, this method has no effect other than to return the existing insight.Transcripts will typically take about 48 hours. When the transcript is ready, an InsightNotification webhook will be POSTed to the bundle notify_url.For more information see Human Transcripts Quick Start.captions_r9 - High-accuracy captions of the speech in video media.Captions will be generated on the first track in the bundle. and are charged per minute (rounded up for partial minutes), based on the duration of the media.  See the pricing page. If the request has already been made, this method has no effect other than to return the existing insight.Captions will typically take about 72 hours. When the captions are ready, an InsightNotification webhook will be POSTed to the bundle notify_url.For more information see Captions Quick Start.spoken_keywords - Spoken words of interest found in audio media. Note: Normally spoken_keywords is set to autorun so you do not need to run it explicitly.spoken_topics - Topics spoken about in the audio media."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "5cf55768-c38c-4de8-9152-220eb189d513"
            }
          ]
        }
      ]
    }
  ]
}