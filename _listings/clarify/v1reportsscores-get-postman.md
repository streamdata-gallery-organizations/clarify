{
  "info": {
    "name": "Clarify Generate Group Report beta",
    "_postman_id": "8aad1851-1353-4ef2-be2d-c5dad27d2ae0",
    "description": "Analyzes bundle content over a series of time periods grouped by the value of group_field metadata field and generates a report of top scores.interval specifies the duration of each time period in the report. For example, you can generate a report that gives monthly statistics. If there are no bundles for a given period, that period will not be present in the report.group_field specifies a metadata field by which to group statistics. Typically the field will represent a user or team id to get a report of the scores for the top users or teams.filter is used to limit the bundles in the report according to specific criteria based on metadata and bundle values.  A report filter behaves in the same way as a search filter. It uses an expression syntax similar to Javascript boolean expressions. An expression is made up of zero or more terms joined by logical operators with each term having a field, a comparison operator, and a literal value.",
    "schema": "https://schema.getpostman.com/json/collection/v2.0.0/"
  },
  "item": [
    {
      "name": "List",
      "item": [
        {
          "id": "57b3a53b-9e33-44ff-a1c7-185a91e4b6c5",
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
              "id": "44d175b9-0fab-45b4-abfe-a0a5fc2f2c35"
            }
          ]
        }
      ]
    },
    {
      "name": "Bundle",
      "item": [
        {
          "id": "e879f410-8c44-4302-abc1-36c53bd74c67",
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
              "id": "313ccc67-d589-4d04-9ba3-1f51ddc9df45"
            }
          ]
        },
        {
          "id": "6d3033db-888e-4b1b-b652-a0703b025b95",
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
              "id": "9605b11c-7a96-4263-8d62-10dafb502fc0"
            }
          ]
        },
        {
          "id": "fe310de4-9edb-4b4b-aacb-75d062441d65",
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
              "id": "3b1cc70b-e763-40aa-8c20-600d93313c74"
            }
          ]
        },
        {
          "id": "c8b817d2-fd79-446c-80ab-c6118deb6df4",
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
              "id": "f5d556af-1295-4498-b344-be9d22f2fbea"
            }
          ]
        },
        {
          "id": "f138175e-1554-4a8a-8d49-cb7f13605ce1",
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
              "id": "684613af-5753-44c6-833b-6233809cdd2c"
            }
          ]
        },
        {
          "id": "ce231b31-44bc-4530-adcd-e23e85e08a30",
          "name": "getV1BundlesBundleInsightsInsight",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/insights/:insight_id"
              ],
              "variable": [
                {
                  "id": "bundle_id",
                  "value": "{}",
                  "type": "string"
                },
                {
                  "id": "insight_id",
                  "value": "{}",
                  "type": "string"
                }
              ]
            },
            "method": "GET",
            "body": {
              "mode": "raw"
            },
            "description": "Gets a particular insight for a bundle. Typically, you will hit this endpoint from a link contained in a response to /v1/bundles/{bundle_id}/insightsThe insight response may contain a data object containing insight-specific data and/or an array of objects called track_data, where the array indexes correspond to the tracks in the bundle. Each object in the array contains the track_id, track_label and insight-specific data related to that insight. For example, in the spoken_words insight, the track_data objects contain the field word_count which is the number of spoken words found in the track.Documentation on the insights available and the data returned can be found at http://docs.clarify.io/insights/Insights that contain data in different file formats (such as for video captions) will have one or more link relations in the _links array for the corresponding data. Note that the href URLs in these links have a limited lifespan and should not be stored locally."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "02c87875-7bce-4387-ac36-c8b053bc55f7"
            }
          ]
        },
        {
          "id": "0f8ea8f2-77bb-4d67-b636-049014fecfc6",
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
              "id": "18e1564e-f3fc-4405-9357-d0dcd512ee07"
            }
          ]
        },
        {
          "id": "dc7f1d8f-0418-480c-9acf-d7c9c0341346",
          "name": "putV1BundlesBundleMetadata",
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
            "method": "PUT",
            "body": {
              "mode": "urlencoded",
              "urlencoded": [
                {
                  "key": "data",
                  "value": "{}",
                  "disabled": false,
                  "description": "User-defined JSON data associated with the bundle"
                },
                {
                  "key": "version",
                  "value": "{}",
                  "disabled": false,
                  "description": "Object version"
                }
              ]
            },
            "description": "Update the metadata for a bundle.The metadata is a single-level JSON object of your own definition, containing key-values that can be searched and filtered on. Metadata can be used to hold text such as names, titles, descriptions and values for segregating bundles, for example by user, topic, folder name etc. The keys (property names) can be up to 64 characters and must contain only alphanumeric characters and underscore (but not start with underscore) and must not be a reserved name. Reserved names are &quot;true&quot;, &quot;false&quot;, and &quot;null&quot;. Values can be strings, numbers, boolean true/false, date-times represented as a string in ISO 8601 format (ex. &quot;2014-02-25T14:23:45.000Z&quot;), or an array of these primitive types. Strings can be up to 2000 characters and strings in arrays can be up to 128 characters each. Nested objects are not allowed. Metadata can contain up to 50 key-value pairs up to a total JSON size of 4000 characters.To clear the metadata for a bundle, send data={}.If version specified, the metadata will only be updated if the current version matches this parameter value. If the version doesn't match, a 409 Conflict will be returned. If version not specified, the metadata will always be updated."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "1a46dde0-2255-4fdb-940c-0cbf3910d6c7"
            }
          ]
        },
        {
          "id": "8a665b13-dc00-4418-85fb-e94a731ded5b",
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
              "id": "108dc1d6-4312-4bf2-ae32-a841659a54e0"
            }
          ]
        },
        {
          "id": "ee4ea44b-0eb9-448c-9b04-1543a2870bd9",
          "name": "getV1BundlesBundleTracks",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/tracks"
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
            "description": "Gets the array of tracks for a bundle. This includes the specification of the media and the status of fetching and processing it.Media for tracks is fetched asynchronously. Until media has been retrieved, a track&apos;s duration and size will both be set to -1."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "90e4905d-2958-4bc2-9315-073fec628e8a"
            }
          ]
        },
        {
          "id": "ab81c0cb-ba40-4c58-98fd-d66c8cdacad8",
          "name": "deleteV1BundlesBundleTracks",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/tracks"
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
            "description": "Delete tracks of a bundle. This will only delete media stored on Clarify systems and not delete the source media on remote systems.Successful response will be a HTTP code 204 with an empty body."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "2e2eaf1f-3577-473a-9ec5-207c40ecbbb3"
            }
          ]
        },
        {
          "id": "38282255-3340-4e51-87ab-559547fb2263",
          "name": "getV1BundlesBundleTracksTrack",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/tracks/:track_id"
              ],
              "variable": [
                {
                  "id": "bundle_id",
                  "value": "{}",
                  "type": "string"
                },
                {
                  "id": "track_id",
                  "value": "{}",
                  "type": "string"
                }
              ]
            },
            "method": "GET",
            "body": {
              "mode": "raw"
            },
            "description": "Gets a single track in a bundle. This includes the specification of the media and the status of fetching and processing it.Media for a track is fetched asynchronously. Until media has been retrieved, a track&apos;s duration and size will both be set to -1."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "6f9f640b-1512-4c8f-90df-e37cfe07c5a5"
            }
          ]
        },
        {
          "id": "7ec170cb-2780-47f0-9870-407f80469c83",
          "name": "deleteV1BundlesBundleTracksTrack",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/tracks/:track_id"
              ],
              "variable": [
                {
                  "id": "bundle_id",
                  "value": "{}",
                  "type": "string"
                },
                {
                  "id": "track_id",
                  "value": "{}",
                  "type": "string"
                }
              ]
            },
            "method": "DELETE",
            "body": {
              "mode": "raw"
            },
            "description": "Delete a track of a bundle. This will only delete media stored on Clarify systems and not delete the source media on remote systems.Successful response will be a HTTP code 204 with an empty body."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "da165c76-bd77-4a42-9442-4ef20d145194"
            }
          ]
        }
      ]
    },
    {
      "name": "Request",
      "item": [
        {
          "id": "4e0695ef-e7c6-49bb-a49d-01e30fa2b8e6",
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
              "id": "bbaa42a4-3514-42b0-8434-cc50aeb8f098"
            }
          ]
        }
      ]
    },
    {
      "name": "Tracksa",
      "item": [
        {
          "id": "d0d74b59-27ed-49e1-8f6a-70ce2ae06be1",
          "name": "putV1BundlesBundleTracks",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/tracks"
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
                  "key": "parts_complete",
                  "value": "{}",
                  "disabled": false,
                  "description": "Set to true if media parts in all tracks are complete"
                },
                {
                  "key": "version",
                  "value": "{}",
                  "disabled": false,
                  "description": "Object version"
                }
              ]
            },
            "description": "Update tracks for a bundle.parts_complete a boolean true or false. If true, any tracks in the PENDING state will be queued for processing and no more media parts may be added to the tracks. Default is false.If version specified, the track will only be updated if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the track will always be updated."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "334595dd-b5cb-458d-8dbc-1010407a88d7"
            }
          ]
        }
      ]
    },
    {
      "name": "Tracka",
      "item": [
        {
          "id": "3e0341a4-7234-4d1e-90d9-32a175d3af90",
          "name": "postV1BundlesBundleTracks",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/tracks"
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
                  "key": "label",
                  "value": "{}",
                  "disabled": false,
                  "description": "Label for the track"
                },
                {
                  "key": "media_url",
                  "value": "{}",
                  "disabled": false,
                  "description": "URL of a media file for this bundle"
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
                },
                {
                  "key": "track",
                  "value": "{}",
                  "disabled": false,
                  "description": "Track number specifies the index of the new track in the tracks array"
                },
                {
                  "key": "version",
                  "value": "{}",
                  "disabled": false,
                  "description": "Object version"
                }
              ]
            },
            "description": "Add a new track to a bundle. This will insert or append a new track in the tracks array or return an error if the maximum number of tracks (12) has been reached or the track number specifies an invalid index.Once all media parts have been added to a track it is immutable, meaning it cannot be modified. If you wish to modify a track, simply add a new one and delete the existing one.label is a short name for the track.media_url must be a publicly accessible url to a media file. It will be fetched asynchronously after the REST call returns. The audio can be mono or stereo.audio_channel is used to specify audio channels if the media is a stereo file. A value of left or right signifies that only the specified channel will be used. If no value or an empty string is specified for audio_channel, all channels will be used in a single track. If your stereo channels were recorded separately with each channel containing distinct content (for example if 2 legs of a phone call were recorded separately and combined into a single stereo file), for best speech recognition, create two tracks with audio_channel to be left and right. If your stereo file is simply a recording made with a stereo microphone, audio_channel should be set to an empty string (or not be specified.)audio_language can be used to specify the language of the audio media. This is an optional parameter and if not specified or an empty string, the language of the track will be automatically detected. If specified, it must be a language code as described in RFC5646 (see http://tools.ietf.org/html/rfc5646). Supported languages: en-US, en-UK, es, fr.start_time a time in seconds that the media starts, relative to start time of the bundle. This allows you to specify sequential parts of media. If not specified, the default is 0.parts_pending a boolean flag specifying if more media parts will subsequently be added to the track. If true, a subsequent API call must be made to signify that the track is complete. If not specified, the default is false.track is the index in the tracks array where the new track will be added. Track numbers start at 0. If this parameter is not specified the new track will always be appended to the end of the array. If the track specified is greater than the last index of the array + 1, an error will be returned.If version specified, the track will only be added if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the track will always be updated."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "2f970851-89d1-4ddc-96a0-bbf7f4e9958f"
            }
          ]
        }
      ]
    },
    {
      "name": "Media",
      "item": [
        {
          "id": "492a9d1d-9719-49af-9c2d-70988a1f39ab",
          "name": "putV1BundlesBundleTracksTrack",
          "request": {
            "url": {
              "protocol": "http",
              "host": "api.clarify.io",
              "path": [
                "v1/bundles/:bundle_id/tracks/:track_id"
              ],
              "variable": [
                {
                  "id": "bundle_id",
                  "value": "{}",
                  "type": "string"
                },
                {
                  "id": "track_id",
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
                  "key": "media_url",
                  "value": "{}",
                  "disabled": false,
                  "description": "URL of a media file for this bundle"
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
                },
                {
                  "key": "version",
                  "value": "{}",
                  "disabled": false,
                  "description": "Object version"
                }
              ]
            },
            "description": "Add media to an existing track of a bundle. This can only be called on a track that currently has no media set or has parts pending.Once all media parts have been added to a track it is immutable, meaning it cannot be modified. If you wish to modify a track, simply add a new one and delete the existing one.media_url must be a publicly accessible url to a media file. It will be fetched asynchronously after the REST call returns. The audio can be mono or stereo.audio_channel is used to specify audio channels if the media is a stereo file. A value of left or right signifies that only the specified channel will be used. If no value or an empty string is specified for audio_channel, all channels will be used in a single track. If your stereo channels were recorded separately with each channel containing distinct content (for example if 2 legs of a phone call were recorded separately and combined into a single stereo file), for best speech recognition, create two tracks with audio_channel to be left and right. If your stereo file is simply a recording made with a stereo microphone, audio_channel should be set to an empty string (or not be specified.)audio_language can be used to specify the language of the audio media. This is an optional parameter and if not specified or an empty string, the language of the track will be automatically detected. If specified, it must be a language code as described in RFC5646 (see http://tools.ietf.org/html/rfc5646). Supported languages: en-US, en-UK, es, fr.start_time a time in seconds that the media starts, relative to start time of the bundle. This allows you to specify sequential parts of media. If not specified, the default is 0.parts_pending a boolean flag specifying if more media parts will subsequently be added to the track. If true, a subsequent API call must be made to signify that the track is complete. If not specified, the default is false.If version specified, the track will only be added if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the track will always be updated."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "eb9e3cd3-d53f-4b10-95c9-e6db5fd83086"
            }
          ]
        }
      ]
    },
    {
      "name": "Generate",
      "item": [
        {
          "id": "57c6525d-cc59-42ac-ab23-8a79c8d09c6c",
          "name": "getV1ReportsScores",
          "request": {
            "url": "http://api.clarify.io/v1/reports/scores?filter=%7B%7D&group_field=%7B%7D&interval=%7B%7D&language=%7B%7D",
            "method": "GET",
            "body": {
              "mode": "raw"
            },
            "description": "Analyzes bundle content over a series of time periods grouped by the value of group_field metadata field and generates a report of top scores.interval specifies the duration of each time period in the report. For example, you can generate a report that gives monthly statistics. If there are no bundles for a given period, that period will not be present in the report.group_field specifies a metadata field by which to group statistics. Typically the field will represent a user or team id to get a report of the scores for the top users or teams.filter is used to limit the bundles in the report according to specific criteria based on metadata and bundle values.  A report filter behaves in the same way as a search filter. It uses an expression syntax similar to Javascript boolean expressions. An expression is made up of zero or more terms joined by logical operators with each term having a field, a comparison operator, and a literal value."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "9e2f25ad-1dd6-4c3d-85fc-670ec8423ccb"
            }
          ]
        }
      ]
    }
  ]
}