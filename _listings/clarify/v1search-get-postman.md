{
  "info": {
    "name": "Clarify Search bundles",
    "_postman_id": "7b7f0134-a9d1-4335-a7fc-272ed8c5f5d0",
    "description": "Searches the bundles and returns a list of matching bundles, along with what matched and where for each bundle.query is used to search for text in the audio and metadata. It uses a simple query language similar to Google. At its simplest, it can be a space separated list of words (ex. open voice) which will find all bundles matching all the words. To search for a phrase, put it in quotes (ex. \"open source\") You can exclude bundles that contain a word by putting a minus (hyphen) in front of the word (ex. -opaque) To search for one word or another, use OR (in uppercase) between the words (ex. pizza OR pasta). As an alternative to OR, you can use | (pipe character).",
    "schema": "https://schema.getpostman.com/json/collection/v2.0.0/"
  },
  "item": [
    {
      "name": "List",
      "item": [
        {
          "id": "4e018c7a-8932-4f97-88dc-67531b2f0fe1",
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
              "id": "8bc839c7-0775-44ee-9cfd-c5f326489bd3"
            }
          ]
        }
      ]
    },
    {
      "name": "Bundle",
      "item": [
        {
          "id": "714e3e7e-96d6-4d00-83c0-a5a679523331",
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
              "id": "2e9f2fa9-d29d-400a-880d-fb7cc7a1ca64"
            }
          ]
        },
        {
          "id": "e72876f5-9fb6-4fcb-9bf0-2b2d0815c6ca",
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
              "id": "eebe6dcf-4263-4a25-a9e5-5f25208fce45"
            }
          ]
        },
        {
          "id": "36b54f74-0d85-4279-aad8-d149b40e9bab",
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
              "id": "f6b8af19-7d78-4ce0-a2a0-af9c6953ee74"
            }
          ]
        },
        {
          "id": "2fc4f1f2-5a2f-4917-a8c9-6401820c59ec",
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
              "id": "4ab3faf1-4d79-4fc3-9365-8c5070a14416"
            }
          ]
        },
        {
          "id": "6eaec346-d59c-4f82-9bc4-b256c4eddd3a",
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
              "id": "2912c301-4121-4faf-b38a-90d72840e24c"
            }
          ]
        },
        {
          "id": "551ec0b3-833b-4823-9780-16d42678b76a",
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
              "id": "f68af0ad-8d61-403c-8240-137f55a55ef0"
            }
          ]
        },
        {
          "id": "d241624d-9b27-4459-841b-7cb355d2f470",
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
              "id": "312011d4-a8f2-4c86-8c51-31fdffdaf1ea"
            }
          ]
        },
        {
          "id": "398f7f4a-3c9a-49e6-a971-0d51e08b57b4",
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
              "id": "88432a68-48f2-47c5-b6e0-756200a56507"
            }
          ]
        },
        {
          "id": "0e6ed71d-538c-4cf5-a04f-be795f1786a2",
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
              "id": "b541cb2a-1a1b-4b77-ba84-4051a2595d85"
            }
          ]
        },
        {
          "id": "f6cb98f1-ccaa-4d34-9bb0-105e3b66edaa",
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
              "id": "8c1a2ab2-8454-4c98-a621-af3b8fac4797"
            }
          ]
        },
        {
          "id": "a0d4f88b-e425-44f0-85bd-7d3f966797cf",
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
              "id": "b5f9e149-49b9-43ad-80a6-530017eac7c3"
            }
          ]
        },
        {
          "id": "e37f6dc3-aec0-448e-8c85-49197216a5eb",
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
              "id": "48d593fd-5e9b-414e-aaf0-45dade7c6cc1"
            }
          ]
        },
        {
          "id": "fffc2151-c2c5-4a20-9148-23a12e4724ef",
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
              "id": "d5ea080b-6bfc-4969-a919-3b98449493ee"
            }
          ]
        }
      ]
    },
    {
      "name": "Request",
      "item": [
        {
          "id": "c4724f53-8318-4e5c-b6c2-d6e4ec29a870",
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
              "id": "e2c6f90c-2101-47f6-bdb6-77025513d935"
            }
          ]
        }
      ]
    },
    {
      "name": "Tracksa",
      "item": [
        {
          "id": "324696e8-bff7-425c-b51a-669567e45100",
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
              "id": "c5d30745-6023-42f5-9e4f-ec6eee85e278"
            }
          ]
        }
      ]
    },
    {
      "name": "Tracka",
      "item": [
        {
          "id": "1bf13e57-353d-40f1-bb74-214e067e852b",
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
              "id": "f0e92900-d0ee-4dbe-abf1-dd57bc6da9f5"
            }
          ]
        }
      ]
    },
    {
      "name": "Media",
      "item": [
        {
          "id": "48393ca8-57b2-41e5-94f9-ee9ec8bac40b",
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
              "id": "dad7e600-a78f-4f37-bdab-740142c2833a"
            }
          ]
        }
      ]
    },
    {
      "name": "Generate",
      "item": [
        {
          "id": "461f84a2-c078-4d19-91b5-bda99797e101",
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
              "id": "9c33b6e7-c361-4c03-b234-ad20ab290dfe"
            }
          ]
        },
        {
          "id": "78606ff1-be62-4840-9fcd-7bf292974885",
          "name": "getV1ReportsTrends",
          "request": {
            "url": "http://api.clarify.io/v1/reports/trends?content=%7B%7D&filter=%7B%7D&interval=%7B%7D&language=%7B%7D",
            "method": "GET",
            "body": {
              "mode": "raw"
            },
            "description": "Analyzes bundle content over a series of time periods and generates a trend report.interval specifies the duration of each time period in the report. For example, you can generate a report that gives monthly statistics. If there are no bundles for a given period, that period will not be present in the report.content specifies the content type to analyze and include in the report. These can include tracks and insights. Multiple values can be specified to generate a rich report. If content is not specified, only bundle counts are included in the report.filter is used to limit the bundles in the report according to specific criteria based on metadata and bundle values.  A report filter behaves in the same way as a search filter."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "ce71bf49-9ce7-4024-8b93-08ea52b073e0"
            }
          ]
        }
      ]
    },
    {
      "name": "Search",
      "item": [
        {
          "id": "a709d4f3-e947-49bc-b79c-c8633bdc9c75",
          "name": "getV1Search",
          "request": {
            "url": "http://api.clarify.io/v1/search?embed=%7B%7D&filter=%7B%7D&iterator=%7B%7D&language=%7B%7D&limit=%7B%7D&query=%7B%7D&query_fields=%7B%7D",
            "method": "GET",
            "body": {
              "mode": "raw"
            },
            "description": "Searches the bundles and returns a list of matching bundles, along with what matched and where for each bundle.query is used to search for text in the audio and metadata. It uses a simple query language similar to Google. At its simplest, it can be a space separated list of words (ex. open voice) which will find all bundles matching all the words. To search for a phrase, put it in quotes (ex. \"open source\") You can exclude bundles that contain a word by putting a minus (hyphen) in front of the word (ex. -opaque) To search for one word or another, use OR (in uppercase) between the words (ex. pizza OR pasta). As an alternative to OR, you can use | (pipe character)."
          },
          "response": [
            {
              "status": "OK",
              "code": 200,
              "name": "Response_200",
              "id": "625f4d14-11a9-4355-b6bb-2dcc5ea91616"
            }
          ]
        }
      ]
    }
  ]
}