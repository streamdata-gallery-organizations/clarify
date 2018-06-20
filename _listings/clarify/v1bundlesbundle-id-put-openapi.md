---
swagger: "2.0"
x-collection-name: Clarify
x-complete: 0
info:
  title: Clarify Update a bundle
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