---
name: Clarify
x-slug: clarify
description: The API to search, re-imagined for a world that???s moved beyond text.
image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
x-kinRank: "9"
x-alexaRank: "2476786"
tags: Clarify
created: "2018-06-20"
modified: "2018-06-20"
url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/apis.md
specificationVersion: "0.14"
apis:
- name: Clarify List bundles
  x-api-slug: clarify
  description: Gets the list of bundles. Links to each item are in the _links with
    link relation items.After getting the initial list, use the first, last, next,
    prev link relations to get more bundles in the list. Note that next will not be
    available at the end of the list and prev will not be available at the start of
    the list. If the results are exactly one page neither prev nor next will be available.The
    embed parameter specifies link relations to embed in the results. The models for
    the specified link relations will be in an array in the embedded object with the
    link relation as the key. For example, if you do embed=items, _embedded will contain
    a property items whose value is the array of bundle models. For link relations
    that are curies (ex. "clarify:metadata"), you may simply use the base name (ex.
    "metadata").
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles
  tags: List,Bundles
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundles-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundles-get-openapi.md
- name: Clarify Create a bundle
  x-api-slug: clarify
  description: 'Create a new bundle with the specified name, media url, and optional
    JSON metadata.name can be any string you wish to associate with the bundle.media_url
    must be a publicly accessible url to a media file. It will be fetched asynchronously
    after the REST call returns. The audio can be mono or stereo.audio_channel is
    used to specify audio channels if the media is a stereo file. A value of left
    or right signifies that only the specified channel will be used. If no value or
    an empty string is specified for audio_channel, all channels will be used in a
    single track. If your stereo channels were recorded separately with each channel
    containing distinct content (for example if 2 legs of a phone call were recorded
    separately and combined into a single stereo file), for best speech recognition,
    create two tracks, with audio_channel set to left and right in each track respectively.
    If your stereo file is simply a recording made with a stereo microphone, audio_channel
    should be set to an empty string (or not be specified.) If you have audio channels
    as separate media files, after creating the bundle with one media_url, POST another
    media_url to /bundles/{bundle_id}/tracks.audio_language can be used to specify
    the language of the audio media. This is an optional parameter and if not specified
    or an empty string, the language of the track will be automatically detected.
    If specified, it must be a language code as described in RFC5646 (see http://tools.ietf.org/html/rfc5646).
    Supported languages: en-US, en-UK, es, fr.label is a short name for the track.metadata
    is a single-level JSON object of your own definition, containing key-values that
    can be searched and filtered on. Metadata can be used to hold text such as names,
    titles, descriptions and values for segregating bundles, for example by user,
    topic, folder name etc. The keys (property names) can be up to 64 characters and
    must contain only alphanumeric characters and underscore (but not start with underscore)
    and must not be a reserved name. Reserved names are &quot;true&quot;, &quot;false&quot;,
    and &quot;null&quot;. Values can be strings, numbers, boolean true/false, date-times
    represented as a string in ISO 8601 format (ex. &quot;2014-02-25T14:23:45.000Z&quot;),
    or an array of these primitive types. Strings can be up to 2000 characters and
    strings in arrays can be up to 128 characters each. Nested objects are not allowed.
    Metadata can contain up to 50 key-value pairs up to a total JSON size of 4000
    characters.start_time a time in seconds that the media starts, relative to start
    time of the bundle. This allows you to specify sequential parts of media. If not
    specified, the default is 0.parts_pending a boolean flag specifying if more media
    parts will subsequently be added to the track. If true, a subsequent API call
    must be made to signify that the track is complete. If not specified, the default
    is false.external_id is an optional parameter that can be used to logically link
    a bundle to an item in an external system. The external_id can be whatever you
    use to identify items in your own database.notify_url is a webhook. It must be
    a publicly accessible url (http or https) on your server to which notifications
    for the bundle will be POSTed. There are three types of notifications: Track Notifications,
    Insight Notifications and Bundle Notifications. For more information on the content
    of notifications and when they are sent, see the notification docs page.If a track
    was created along with the budle, the link relation clarify:track will be included
    with a link to the new track.'
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles
  tags: Bundle
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundles-post-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundles-post-openapi.md
- name: Clarify Delete a bundle
  x-api-slug: clarify
  description: Delete a bundle and its related metadata and tracks. This will only
    delete media stored on Clarify systems and not delete the source media on remote
    systems.Successful response will be a HTTP code 204 with an empty body.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}
  tags: Bundle
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-id-delete-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-id-delete-openapi.md
- name: Clarify Get a bundle
  x-api-slug: clarify
  description: Get a bundle that has previously been created.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}
  tags: Bundle
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-id-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-id-get-openapi.md
- name: Clarify Update a bundle
  x-api-slug: clarify
  description: 'Update a bundle. To update the tracks, media, or metadata of a bundle,
    use the tracks and metadata endpoints.name can be any string you wish to associate
    with the bundle.external_id is an optional parameter that can be used to logically
    link a bundle to an item in an external system. The external_id can be whatever
    you use to identify items in your own database.notify_url is a webhook. It must
    be a publicly accessible url (http or https) on your server to which notifications
    for the bundle will be POSTed. There are three types of notifications: Track Notifications,
    Insight Notifications and Bundle Notifications. For more information on the content
    of notifications and when they are sent, see the notification docs page.If version
    is specified, the bundle will only be updated if the current version matches this
    parameter value. If the version doesn''t match, a 409 Conflict error will be returned.
    If version not specified, the bundle will always be updated.'
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}
  tags: Bundle
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-id-put-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-id-put-openapi.md
- name: Clarify Get bundle insights
  x-api-slug: clarify
  description: Gets the insights for a bundle.URLs of the available insights for the
    bundle are in the _links object, with the link relations (keys) of the format
    insight:insight_name.Documentation on the insights available and the data returned
    can be found at http://docs.clarify.io/insights/
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/insights
  tags: Bundle,Insights
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idinsights-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idinsights-get-openapi.md
- name: Clarify Request an insight to be run
  x-api-slug: clarify
  description: 'Request an insight to be run on a bundle. Note that most insights
    are set to automatically run on all bundles so you commonly won&apos;t need to
    call this endpoint except to request transcripts. To configure which insights
    are automatically run for an app, visit the Clarify Developer Portal. Insights
    that are not configured to autorun can be requested to run on an individual bundle
    using this endpoint. The following insights can be requested:transcript_r9 - High-accuracy
    transcript of the speech in audio media.Transcripts will produced on the mixed
    audio of all tracks in the bundle and are charged per minute (rounded up for partial
    minutes), based on the duration of the longest track. If the request has already
    been made, this method has no effect other than to return the existing insight.Transcripts
    will typically take about 48 hours. When the transcript is ready, an InsightNotification
    webhook will be POSTed to the bundle notify_url.For more information see Human
    Transcripts Quick Start.captions_r9 - High-accuracy captions of the speech in
    video media.Captions will be generated on the first track in the bundle. and are
    charged per minute (rounded up for partial minutes), based on the duration of
    the media.  See the pricing page. If the request has already been made, this method
    has no effect other than to return the existing insight.Captions will typically
    take about 72 hours. When the captions are ready, an InsightNotification webhook
    will be POSTed to the bundle notify_url.For more information see Captions Quick
    Start.spoken_keywords - Spoken words of interest found in audio media. Note: Normally
    spoken_keywords is set to autorun so you do not need to run it explicitly.spoken_topics
    - Topics spoken about in the audio media.'
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/insights
  tags: Request,Insight,To,Be,Run
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idinsights-post-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idinsights-post-openapi.md
- name: Clarify Get bundle insight
  x-api-slug: clarify
  description: Gets a particular insight for a bundle. Typically, you will hit this
    endpoint from a link contained in a response to /v1/bundles/{bundle_id}/insightsThe
    insight response may contain a data object containing insight-specific data and/or
    an array of objects called track_data, where the array indexes correspond to the
    tracks in the bundle. Each object in the array contains the track_id, track_label
    and insight-specific data related to that insight. For example, in the spoken_words
    insight, the track_data objects contain the field word_count which is the number
    of spoken words found in the track.Documentation on the insights available and
    the data returned can be found at http://docs.clarify.io/insights/Insights that
    contain data in different file formats (such as for video captions) will have
    one or more link relations in the _links array for the corresponding data. Note
    that the href URLs in these links have a limited lifespan and should not be stored
    locally.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/insights/{insight_id}
  tags: Bundle,Insight
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idinsightsinsight-id-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idinsightsinsight-id-get-openapi.md
- name: Clarify Delete bundle metadata
  x-api-slug: clarify
  description: Delete the metadata of a bundle and set data to {} (empty object.)
    This is functionally equivalent to an update metadata request with data set to
    {}.Successful response will be a HTTP code 204 with an empty body.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/metadata
  tags: Bundle,Metadata
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idmetadata-delete-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idmetadata-delete-openapi.md
- name: Clarify Get bundle metadata
  x-api-slug: clarify
  description: Gets the metadata for a bundle.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/metadata
  tags: Bundle,Metadata
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idmetadata-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idmetadata-get-openapi.md
- name: Clarify Update bundle metadata
  x-api-slug: clarify
  description: Update the metadata for a bundle.The metadata is a single-level JSON
    object of your own definition, containing key-values that can be searched and
    filtered on. Metadata can be used to hold text such as names, titles, descriptions
    and values for segregating bundles, for example by user, topic, folder name etc.
    The keys (property names) can be up to 64 characters and must contain only alphanumeric
    characters and underscore (but not start with underscore) and must not be a reserved
    name. Reserved names are &quot;true&quot;, &quot;false&quot;, and &quot;null&quot;.
    Values can be strings, numbers, boolean true/false, date-times represented as
    a string in ISO 8601 format (ex. &quot;2014-02-25T14:23:45.000Z&quot;), or an
    array of these primitive types. Strings can be up to 2000 characters and strings
    in arrays can be up to 128 characters each. Nested objects are not allowed. Metadata
    can contain up to 50 key-value pairs up to a total JSON size of 4000 characters.To
    clear the metadata for a bundle, send data={}.If version specified, the metadata
    will only be updated if the current version matches this parameter value. If the
    version doesn't match, a 409 Conflict will be returned. If version not specified,
    the metadata will always be updated.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/metadata
  tags: Bundle,Metadata
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idmetadata-put-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idmetadata-put-openapi.md
- name: Clarify Delete bundle tracks
  x-api-slug: clarify
  description: Delete tracks of a bundle. This will only delete media stored on Clarify
    systems and not delete the source media on remote systems.Successful response
    will be a HTTP code 204 with an empty body.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/tracks
  tags: Bundle,Tracks
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtracks-delete-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtracks-delete-openapi.md
- name: Clarify Get bundle tracks
  x-api-slug: clarify
  description: Gets the array of tracks for a bundle. This includes the specification
    of the media and the status of fetching and processing it.Media for tracks is
    fetched asynchronously. Until media has been retrieved, a track&apos;s duration
    and size will both be set to -1.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/tracks
  tags: Bundle,Tracks
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtracks-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtracks-get-openapi.md
- name: Clarify Add a track for a bundle
  x-api-slug: clarify
  description: 'Add a new track to a bundle. This will insert or append a new track
    in the tracks array or return an error if the maximum number of tracks (12) has
    been reached or the track number specifies an invalid index.Once all media parts
    have been added to a track it is immutable, meaning it cannot be modified. If
    you wish to modify a track, simply add a new one and delete the existing one.label
    is a short name for the track.media_url must be a publicly accessible url to a
    media file. It will be fetched asynchronously after the REST call returns. The
    audio can be mono or stereo.audio_channel is used to specify audio channels if
    the media is a stereo file. A value of left or right signifies that only the specified
    channel will be used. If no value or an empty string is specified for audio_channel,
    all channels will be used in a single track. If your stereo channels were recorded
    separately with each channel containing distinct content (for example if 2 legs
    of a phone call were recorded separately and combined into a single stereo file),
    for best speech recognition, create two tracks with audio_channel to be left and
    right. If your stereo file is simply a recording made with a stereo microphone,
    audio_channel should be set to an empty string (or not be specified.)audio_language
    can be used to specify the language of the audio media. This is an optional parameter
    and if not specified or an empty string, the language of the track will be automatically
    detected. If specified, it must be a language code as described in RFC5646 (see
    http://tools.ietf.org/html/rfc5646). Supported languages: en-US, en-UK, es, fr.start_time
    a time in seconds that the media starts, relative to start time of the bundle.
    This allows you to specify sequential parts of media. If not specified, the default
    is 0.parts_pending a boolean flag specifying if more media parts will subsequently
    be added to the track. If true, a subsequent API call must be made to signify
    that the track is complete. If not specified, the default is false.track is the
    index in the tracks array where the new track will be added. Track numbers start
    at 0. If this parameter is not specified the new track will always be appended
    to the end of the array. If the track specified is greater than the last index
    of the array + 1, an error will be returned.If version specified, the track will
    only be added if the current version matches this parameter value. If the version
    doesn''t match, a 409 Conflict error will be returned. If version not specified,
    the track will always be updated.'
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/tracks
  tags: Tracka,Bundle
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtracks-post-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtracks-post-openapi.md
- name: Clarify Update a tracks for a bundle
  x-api-slug: clarify
  description: Update tracks for a bundle.parts_complete a boolean true or false.
    If true, any tracks in the PENDING state will be queued for processing and no
    more media parts may be added to the tracks. Default is false.If version specified,
    the track will only be updated if the current version matches this parameter value.
    If the version doesn't match, a 409 Conflict error will be returned. If version
    not specified, the track will always be updated.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/tracks
  tags: Tracksa,Bundle
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtracks-put-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtracks-put-openapi.md
- name: Clarify Delete a bundle track
  x-api-slug: clarify
  description: Delete a track of a bundle. This will only delete media stored on Clarify
    systems and not delete the source media on remote systems.Successful response
    will be a HTTP code 204 with an empty body.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/tracks/{track_id}
  tags: Bundle,Track
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtrackstrack-id-delete-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtrackstrack-id-delete-openapi.md
- name: Clarify Get bundle track
  x-api-slug: clarify
  description: Gets a single track in a bundle. This includes the specification of
    the media and the status of fetching and processing it.Media for a track is fetched
    asynchronously. Until media has been retrieved, a track&apos;s duration and size
    will both be set to -1.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/tracks/{track_id}
  tags: Bundle,Track
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtrackstrack-id-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtrackstrack-id-get-openapi.md
- name: Clarify Add media to a track
  x-api-slug: clarify
  description: 'Add media to an existing track of a bundle. This can only be called
    on a track that currently has no media set or has parts pending.Once all media
    parts have been added to a track it is immutable, meaning it cannot be modified.
    If you wish to modify a track, simply add a new one and delete the existing one.media_url
    must be a publicly accessible url to a media file. It will be fetched asynchronously
    after the REST call returns. The audio can be mono or stereo.audio_channel is
    used to specify audio channels if the media is a stereo file. A value of left
    or right signifies that only the specified channel will be used. If no value or
    an empty string is specified for audio_channel, all channels will be used in a
    single track. If your stereo channels were recorded separately with each channel
    containing distinct content (for example if 2 legs of a phone call were recorded
    separately and combined into a single stereo file), for best speech recognition,
    create two tracks with audio_channel to be left and right. If your stereo file
    is simply a recording made with a stereo microphone, audio_channel should be set
    to an empty string (or not be specified.)audio_language can be used to specify
    the language of the audio media. This is an optional parameter and if not specified
    or an empty string, the language of the track will be automatically detected.
    If specified, it must be a language code as described in RFC5646 (see http://tools.ietf.org/html/rfc5646).
    Supported languages: en-US, en-UK, es, fr.start_time a time in seconds that the
    media starts, relative to start time of the bundle. This allows you to specify
    sequential parts of media. If not specified, the default is 0.parts_pending a
    boolean flag specifying if more media parts will subsequently be added to the
    track. If true, a subsequent API call must be made to signify that the track is
    complete. If not specified, the default is false.If version specified, the track
    will only be added if the current version matches this parameter value. If the
    version doesn''t match, a 409 Conflict error will be returned. If version not
    specified, the track will always be updated.'
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/bundles/{bundle_id}/tracks/{track_id}
  tags: Media,To,Track
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtrackstrack-id-put-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1bundlesbundle-idtrackstrack-id-put-openapi.md
- name: Clarify Generate Group Report beta
  x-api-slug: clarify
  description: Analyzes bundle content over a series of time periods grouped by the
    value of group_field metadata field and generates a report of top scores.interval
    specifies the duration of each time period in the report. For example, you can
    generate a report that gives monthly statistics. If there are no bundles for a
    given period, that period will not be present in the report.group_field specifies
    a metadata field by which to group statistics. Typically the field will represent
    a user or team id to get a report of the scores for the top users or teams.filter
    is used to limit the bundles in the report according to specific criteria based
    on metadata and bundle values.  A report filter behaves in the same way as a search
    filter. It uses an expression syntax similar to Javascript boolean expressions.
    An expression is made up of zero or more terms joined by logical operators with
    each term having a field, a comparison operator, and a literal value.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/reports/scores
  tags: Generate,Group,Report,Beta
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1reportsscores-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1reportsscores-get-openapi.md
- name: Clarify Generate Trends Report beta
  x-api-slug: clarify
  description: Analyzes bundle content over a series of time periods and generates
    a trend report.interval specifies the duration of each time period in the report.
    For example, you can generate a report that gives monthly statistics. If there
    are no bundles for a given period, that period will not be present in the report.content
    specifies the content type to analyze and include in the report. These can include
    tracks and insights. Multiple values can be specified to generate a rich report.
    If content is not specified, only bundle counts are included in the report.filter
    is used to limit the bundles in the report according to specific criteria based
    on metadata and bundle values.  A report filter behaves in the same way as a search
    filter.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/reports/trends
  tags: Generate,Trends,Report,Beta
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1reportstrends-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1reportstrends-get-openapi.md
- name: Clarify Search bundles
  x-api-slug: clarify
  description: Searches the bundles and returns a list of matching bundles, along
    with what matched and where for each bundle.query is used to search for text in
    the audio and metadata. It uses a simple query language similar to Google. At
    its simplest, it can be a space separated list of words (ex. open voice) which
    will find all bundles matching all the words. To search for a phrase, put it in
    quotes (ex. "open source") You can exclude bundles that contain a word by putting
    a minus (hyphen) in front of the word (ex. -opaque) To search for one word or
    another, use OR (in uppercase) between the words (ex. pizza OR pasta). As an alternative
    to OR, you can use | (pipe character).
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io////v1/search
  tags: Search,Bundles
  properties:
  - type: x-postman-collection
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1search-get-postman.md
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/v1search-get-openapi.md
- name: Clarify
  x-api-slug: clarify
  description: The API to search, re-imagined for a world that???s moved beyond text.
  image: http://kinlane-productions.s3.amazonaws.com/screen-capture-api/3503-clarify.jpg
  humanURL: http://clarify.io/
  baseURL: https://api.clarify.io//
  tags: Clarify
  properties:
  - type: x-openapi-spec
    url: https://raw.githubusercontent.com/streamdata-gallery-organizations/clarify/master/_listings/clarify/openapi.md
x-common:
- type: x-base
  url: https://api.clarify.io
- type: x-blog
  url: http://clarify.io/blog/
- type: x-crunchbase
  url: https://crunchbase.com/organization/clarify-inc
- type: x-developer
  url: http://developer.clarify.io/
- type: x-documentation
  url: http://clarify.io/docs/api/
- type: x-email
  url: contact@clarify.io
- type: x-email
  url: cfy.dmca@clarify.io
- type: x-github
  url: https://github.com/OP3Nvoice
- type: x-pricing
  url: http://clarify.io/pricing/
- type: x-twitter
  url: https://twitter.com/ClarifyAPI
- type: x-twitter
  url: https://twitter.com/clarifyio
- type: x-website
  url: http://clarify.io/
- type: x-website
  url: http://clarify.io
include: []
maintainers:
- FN: Kin Lane
  x-twitter: apievangelist
  email: info@apievangelist.com
---