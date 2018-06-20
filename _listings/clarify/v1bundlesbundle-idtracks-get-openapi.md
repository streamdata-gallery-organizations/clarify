---
swagger: "2.0"
x-collection-name: Clarify
x-complete: 0
info:
  title: Clarify Get bundle tracks
  description: Gets the array of tracks for a bundle. This includes the specification
    of the media and the status of fetching and processing it.Media for tracks is
    fetched asynchronously. Until media has been retrieved, a track&apos;s duration
    and size will both be set to -1.
  version: 1.3.4
host: api.clarify.io
basePath: /
schemes:
- http
produces:
- application/json
consumes:
- application/json
paths:
  /v1/bundles:
    get:
      summary: List bundles
      description: Gets the list of bundles. Links to each item are in the _links
        with link relation items.After getting the initial list, use the first, last,
        next, prev link relations to get more bundles in the list. Note that next
        will not be available at the end of the list and prev will not be available
        at the start of the list. If the results are exactly one page neither prev
        nor next will be available.The embed parameter specifies link relations to
        embed in the results. The models for the specified link relations will be
        in an array in the embedded object with the link relation as the key. For
        example, if you do embed=items, _embedded will contain a property items whose
        value is the array of bundle models. For link relations that are curies (ex.
        "clarify:metadata"), you may simply use the base name (ex. "metadata").
      operationId: getV1Bundles
      x-api-path-slug: v1bundles-get
      parameters:
      - in: query
        name: embed
        description: list of link relations to embed in the result collection
      - in: query
        name: iterator
        description: optional opaque value, automatically provided in next/prev links,
          or literal first, last
      - in: query
        name: limit
        description: limit results to specified number of bundles
      responses:
        200:
          description: OK
      tags:
      - List
      - Bundles
    post:
      summary: Create a bundle
      description: 'Create a new bundle with the specified name, media url, and optional
        JSON metadata.name can be any string you wish to associate with the bundle.media_url
        must be a publicly accessible url to a media file. It will be fetched asynchronously
        after the REST call returns. The audio can be mono or stereo.audio_channel
        is used to specify audio channels if the media is a stereo file. A value of
        left or right signifies that only the specified channel will be used. If no
        value or an empty string is specified for audio_channel, all channels will
        be used in a single track. If your stereo channels were recorded separately
        with each channel containing distinct content (for example if 2 legs of a
        phone call were recorded separately and combined into a single stereo file),
        for best speech recognition, create two tracks, with audio_channel set to
        left and right in each track respectively. If your stereo file is simply a
        recording made with a stereo microphone, audio_channel should be set to an
        empty string (or not be specified.) If you have audio channels as separate
        media files, after creating the bundle with one media_url, POST another media_url
        to /bundles/{bundle_id}/tracks.audio_language can be used to specify the language
        of the audio media. This is an optional parameter and if not specified or
        an empty string, the language of the track will be automatically detected.
        If specified, it must be a language code as described in RFC5646 (see http://tools.ietf.org/html/rfc5646).
        Supported languages: en-US, en-UK, es, fr.label is a short name for the track.metadata
        is a single-level JSON object of your own definition, containing key-values
        that can be searched and filtered on. Metadata can be used to hold text such
        as names, titles, descriptions and values for segregating bundles, for example
        by user, topic, folder name etc. The keys (property names) can be up to 64
        characters and must contain only alphanumeric characters and underscore (but
        not start with underscore) and must not be a reserved name. Reserved names
        are &quot;true&quot;, &quot;false&quot;, and &quot;null&quot;. Values can
        be strings, numbers, boolean true/false, date-times represented as a string
        in ISO 8601 format (ex. &quot;2014-02-25T14:23:45.000Z&quot;), or an array
        of these primitive types. Strings can be up to 2000 characters and strings
        in arrays can be up to 128 characters each. Nested objects are not allowed.
        Metadata can contain up to 50 key-value pairs up to a total JSON size of 4000
        characters.start_time a time in seconds that the media starts, relative to
        start time of the bundle. This allows you to specify sequential parts of media.
        If not specified, the default is 0.parts_pending a boolean flag specifying
        if more media parts will subsequently be added to the track. If true, a subsequent
        API call must be made to signify that the track is complete. If not specified,
        the default is false.external_id is an optional parameter that can be used
        to logically link a bundle to an item in an external system. The external_id
        can be whatever you use to identify items in your own database.notify_url
        is a webhook. It must be a publicly accessible url (http or https) on your
        server to which notifications for the bundle will be POSTed. There are three
        types of notifications: Track Notifications, Insight Notifications and Bundle
        Notifications. For more information on the content of notifications and when
        they are sent, see the notification docs page.If a track was created along
        with the budle, the link relation clarify:track will be included with a link
        to the new track.'
      operationId: postV1Bundles
      x-api-path-slug: v1bundles-post
      parameters:
      - in: formData
        name: audio_channel
        description: The audio channel to use for the track (  | left | right )
      - in: formData
        name: audio_language
        description: Language of the audio in the track, specified with an RFC5646
          code
      - in: formData
        name: external_id
        description: A string that can refer to an item in an external system
      - in: formData
        name: label
        description: Label for the track (if media_url is specified
      - in: formData
        name: media_url
        description: URL of a media (audio or video) file for this bundle
      - in: formData
        name: metadata
        description: User-defined JSON data associated with the bundle
      - in: formData
        name: name
        description: Name of the bundle
      - in: formData
        name: notify_url
        description: URL for notifications on this bundle
      - in: formData
        name: parts_pending
        description: Set to true if more media parts will be added to the track
      - in: formData
        name: start_time
        description: Time offset in seconds that the media starts relative to the
          bundle
      responses:
        200:
          description: OK
      tags:
      - Bundle
  /v1/bundles/{bundle_id}:
    delete:
      summary: Delete a bundle
      description: Delete a bundle and its related metadata and tracks. This will
        only delete media stored on Clarify systems and not delete the source media
        on remote systems.Successful response will be a HTTP code 204 with an empty
        body.
      operationId: deleteV1BundlesBundle
      x-api-path-slug: v1bundlesbundle-id-delete
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      responses:
        200:
          description: OK
      tags:
      - Bundle
    get:
      summary: Get a bundle
      description: Get a bundle that has previously been created.
      operationId: getV1BundlesBundle
      x-api-path-slug: v1bundlesbundle-id-get
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      - in: query
        name: embed
        description: list of link relations to embed in the result bundle
      responses:
        200:
          description: OK
      tags:
      - Bundle
    put:
      summary: Update a bundle
      description: 'Update a bundle. To update the tracks, media, or metadata of a
        bundle, use the tracks and metadata endpoints.name can be any string you wish
        to associate with the bundle.external_id is an optional parameter that can
        be used to logically link a bundle to an item in an external system. The external_id
        can be whatever you use to identify items in your own database.notify_url
        is a webhook. It must be a publicly accessible url (http or https) on your
        server to which notifications for the bundle will be POSTed. There are three
        types of notifications: Track Notifications, Insight Notifications and Bundle
        Notifications. For more information on the content of notifications and when
        they are sent, see the notification docs page.If version is specified, the
        bundle will only be updated if the current version matches this parameter
        value. If the version doesn''t match, a 409 Conflict error will be returned.
        If version not specified, the bundle will always be updated.'
      operationId: putV1BundlesBundle
      x-api-path-slug: v1bundlesbundle-id-put
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      - in: formData
        name: external_id
        description: A string that can refer to an item in an external system
      - in: formData
        name: name
        description: Name of the bundle
      - in: formData
        name: notify_url
        description: URL for notifications on this bundle
      - in: formData
        name: version
        description: Object version
      responses:
        200:
          description: OK
      tags:
      - Bundle
  /v1/bundles/{bundle_id}/insights:
    get:
      summary: Get bundle insights
      description: Gets the insights for a bundle.URLs of the available insights for
        the bundle are in the _links object, with the link relations (keys) of the
        format insight:insight_name.Documentation on the insights available and the
        data returned can be found at http://docs.clarify.io/insights/
      operationId: getV1BundlesBundleInsights
      x-api-path-slug: v1bundlesbundle-idinsights-get
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      responses:
        200:
          description: OK
      tags:
      - Bundle
      - Insights
    post:
      summary: Request an insight to be run
      description: 'Request an insight to be run on a bundle. Note that most insights
        are set to automatically run on all bundles so you commonly won&apos;t need
        to call this endpoint except to request transcripts. To configure which insights
        are automatically run for an app, visit the Clarify Developer Portal. Insights
        that are not configured to autorun can be requested to run on an individual
        bundle using this endpoint. The following insights can be requested:transcript_r9
        - High-accuracy transcript of the speech in audio media.Transcripts will produced
        on the mixed audio of all tracks in the bundle and are charged per minute
        (rounded up for partial minutes), based on the duration of the longest track.
        If the request has already been made, this method has no effect other than
        to return the existing insight.Transcripts will typically take about 48 hours.
        When the transcript is ready, an InsightNotification webhook will be POSTed
        to the bundle notify_url.For more information see Human Transcripts Quick
        Start.captions_r9 - High-accuracy captions of the speech in video media.Captions
        will be generated on the first track in the bundle. and are charged per minute
        (rounded up for partial minutes), based on the duration of the media.  See
        the pricing page. If the request has already been made, this method has no
        effect other than to return the existing insight.Captions will typically take
        about 72 hours. When the captions are ready, an InsightNotification webhook
        will be POSTed to the bundle notify_url.For more information see Captions
        Quick Start.spoken_keywords - Spoken words of interest found in audio media.
        Note: Normally spoken_keywords is set to autorun so you do not need to run
        it explicitly.spoken_topics - Topics spoken about in the audio media.'
      operationId: postV1BundlesBundleInsights
      x-api-path-slug: v1bundlesbundle-idinsights-post
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      - in: formData
        name: insight
        description: 'name of the insight: transcript_r9, captions_r9, spoken_keywords,
          spoken_topics, spoken_words'
      responses:
        200:
          description: OK
      tags:
      - Request
      - Insight
      - To
      - Be
      - Run
  /v1/bundles/{bundle_id}/insights/{insight_id}:
    get:
      summary: Get bundle insight
      description: Gets a particular insight for a bundle. Typically, you will hit
        this endpoint from a link contained in a response to /v1/bundles/{bundle_id}/insightsThe
        insight response may contain a data object containing insight-specific data
        and/or an array of objects called track_data, where the array indexes correspond
        to the tracks in the bundle. Each object in the array contains the track_id,
        track_label and insight-specific data related to that insight. For example,
        in the spoken_words insight, the track_data objects contain the field word_count
        which is the number of spoken words found in the track.Documentation on the
        insights available and the data returned can be found at http://docs.clarify.io/insights/Insights
        that contain data in different file formats (such as for video captions) will
        have one or more link relations in the _links array for the corresponding
        data. Note that the href URLs in these links have a limited lifespan and should
        not be stored locally.
      operationId: getV1BundlesBundleInsightsInsight
      x-api-path-slug: v1bundlesbundle-idinsightsinsight-id-get
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      - in: path
        name: insight_id
        description: id of an insight
      responses:
        200:
          description: OK
      tags:
      - Bundle
      - Insight
  /v1/bundles/{bundle_id}/metadata:
    delete:
      summary: Delete bundle metadata
      description: Delete the metadata of a bundle and set data to {} (empty object.)
        This is functionally equivalent to an update metadata request with data set
        to {}.Successful response will be a HTTP code 204 with an empty body.
      operationId: deleteV1BundlesBundleMetadata
      x-api-path-slug: v1bundlesbundle-idmetadata-delete
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      responses:
        200:
          description: OK
      tags:
      - Bundle
      - Metadata
    get:
      summary: Get bundle metadata
      description: Gets the metadata for a bundle.
      operationId: getV1BundlesBundleMetadata
      x-api-path-slug: v1bundlesbundle-idmetadata-get
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      responses:
        200:
          description: OK
      tags:
      - Bundle
      - Metadata
    put:
      summary: Update bundle metadata
      description: Update the metadata for a bundle.The metadata is a single-level
        JSON object of your own definition, containing key-values that can be searched
        and filtered on. Metadata can be used to hold text such as names, titles,
        descriptions and values for segregating bundles, for example by user, topic,
        folder name etc. The keys (property names) can be up to 64 characters and
        must contain only alphanumeric characters and underscore (but not start with
        underscore) and must not be a reserved name. Reserved names are &quot;true&quot;,
        &quot;false&quot;, and &quot;null&quot;. Values can be strings, numbers, boolean
        true/false, date-times represented as a string in ISO 8601 format (ex. &quot;2014-02-25T14:23:45.000Z&quot;),
        or an array of these primitive types. Strings can be up to 2000 characters
        and strings in arrays can be up to 128 characters each. Nested objects are
        not allowed. Metadata can contain up to 50 key-value pairs up to a total JSON
        size of 4000 characters.To clear the metadata for a bundle, send data={}.If
        version specified, the metadata will only be updated if the current version
        matches this parameter value. If the version doesn't match, a 409 Conflict
        will be returned. If version not specified, the metadata will always be updated.
      operationId: putV1BundlesBundleMetadata
      x-api-path-slug: v1bundlesbundle-idmetadata-put
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      - in: formData
        name: data
        description: User-defined JSON data associated with the bundle
      - in: formData
        name: version
        description: Object version
      responses:
        200:
          description: OK
      tags:
      - Bundle
      - Metadata
  /v1/bundles/{bundle_id}/tracks:
    delete:
      summary: Delete bundle tracks
      description: Delete tracks of a bundle. This will only delete media stored on
        Clarify systems and not delete the source media on remote systems.Successful
        response will be a HTTP code 204 with an empty body.
      operationId: deleteV1BundlesBundleTracks
      x-api-path-slug: v1bundlesbundle-idtracks-delete
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      responses:
        200:
          description: OK
      tags:
      - Bundle
      - Tracks
    get:
      summary: Get bundle tracks
      description: Gets the array of tracks for a bundle. This includes the specification
        of the media and the status of fetching and processing it.Media for tracks
        is fetched asynchronously. Until media has been retrieved, a track&apos;s
        duration and size will both be set to -1.
      operationId: getV1BundlesBundleTracks
      x-api-path-slug: v1bundlesbundle-idtracks-get
      parameters:
      - in: path
        name: bundle_id
        description: id of a bundle
      responses:
        200:
          description: OK
      tags:
      - Bundle
      - Tracks
x-streamrank:
  polling_total_time_average: 0
  polling_size_download_average: 0
  streaming_total_time_average: 0
  streaming_size_download_average: 0
  change_yes: 0
  change_no: 0
  time_percentage: 0
  size_percentage: 0
  change_percentage: 0
  last_run: ""
  days_run: 0
  minute_run: 0
---