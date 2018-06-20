{
  "info": {
    "name": "Clarify Add media to a track",
    "_postman_id": "92cbd7a7-01ca-4c18-ab82-02535e10c909",
    "description": "Add media to an existing track of a bundle. This can only be called on a track that currently has no media set or has parts pending.Once all media parts have been added to a track it is immutable, meaning it cannot be modified. If you wish to modify a track, simply add a new one and delete the existing one.media_url must be a publicly accessible url to a media file. It will be fetched asynchronously after the REST call returns. The audio can be mono or stereo.audio_channel is used to specify audio channels if the media is a stereo file. A value of left or right signifies that only the specified channel will be used. If no value or an empty string is specified for audio_channel, all channels will be used in a single track. If your stereo channels were recorded separately with each channel containing distinct content (for example if 2 legs of a phone call were recorded separately and combined into a single stereo file), for best speech recognition, create two tracks with audio_channel to be left and right. If your stereo file is simply a recording made with a stereo microphone, audio_channel should be set to an empty string (or not be specified.)audio_language can be used to specify the language of the audio media. This is an optional parameter and if not specified or an empty string, the language of the track will be automatically detected. If specified, it must be a language code as described in RFC5646 (see http://tools.ietf.org/html/rfc5646). Supported languages: en-US, en-UK, es, fr.start_time a time in seconds that the media starts, relative to start time of the bundle. This allows you to specify sequential parts of media. If not specified, the default is 0.parts_pending a boolean flag specifying if more media parts will subsequently be added to the track. If true, a subsequent API call must be made to signify that the track is complete. If not specified, the default is false.If version specified, the track will only be added if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the track will always be updated.",
    "schema": "https://schema.getpostman.com/json/collection/v2.0.0/"
  },
  "item": [
    {
      "name": "Media",
      "item": [
        {
          "id": "39ed2a82-b320-4d6c-a785-07e9a11ae0f8",
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
              "id": "dbb3d1d4-07cb-4d88-8056-46346bd71f0c"
            }
          ]
        }
      ]
    }
  ]
}