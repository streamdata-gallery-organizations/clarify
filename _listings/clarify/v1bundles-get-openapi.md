---
swagger: "2.0"
x-collection-name: Clarify
x-complete: 0
info:
  title: Clarify List bundles
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