Calendar-API-Spec
=================

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt

## Base URL

All URLs references in this documentation have the base

    http://api.guggenheim.org/calendar/

## Making Requests

All requests are made over HTTP. Authentication (by access key) is required and
the client must explicitly accept the 
`application/vnd.guggenheim.calendar+json` content-type.

All responses are JSON.

Only GET requests are accepted, any other kind of request will result in a
405 Method Not Allowed error.

### Authentication

The Guggenheim Calendar API requires an access key. You key can provided 
either via a header (`X-GUGGENHEIM-API-KEY`)or as a query parameter (`key`). 
For example, using curl, the header method would look like:

    curl -H "X-GUGGENHEIM-API-KEY: [YOUR_KEY]" http://api.guggenheim.org/calendar/

using a query parameter:

    curl http://api.guggenheim.org/calendar/?key=[YOUR_KEY]

The two are equivalent. The header method is preferred. A missing or invalid key will result in a 401 Unauthorized error

### Content Type

All responses are JSON, in the `application/vnd.guggenheim.calendar+json` 
media type. Clients must explicitly accept this media type by sending the
appropriate `Accept` header. In curl:

    curl -H "Accept: application/vnd.guggenheim.calendar+json" \
        -H "X-GUGGENHEIM-API-KEY: [YOUR_KEY]" http://api.guggenheim.org/calendar/

A missing or incorrect `Accept` header will result in a 406 Not Acceptable 
error.

## Endpoints

<table>
    <thead>
        <th>Endpoint</th>
        <th>Description</th>
    </thead>
    <tbody>
        <tr>
            <td>calendar/</td>
            <td>Index of available resources</td>
        </tr>
        <tr>
            <td>calendar/events/{id}</td>
            <td>One Event</td>
        </tr>
        <tr>
            <td>calendar/instances</td>
            <td>List of Instances on the current day</td>
        </tr>
        <tr>
            <td>calendar/instances/{YYYY-MM-DD}</td>
            <td>List of Instances on the specified day</td>
        </tr>
        <tr>
            <td>calendar/instances/{YYYY-MM}</td>
            <td>List of Instances in the specified month</td>
        </tr>
        <tr>
            <td>calendar/instances/{id}</td>
            <td>One Instance</td>
        </tr>
        <tr>
            <td>calendar/categories</td>
            <td>List of Categories</td>
        </tr>
        <tr>
            <td>calendar/categories/{id}</td>
            <td>One Category</td>
        </tr>
    </tbody>
</table>

The root endpoint will simply return a list of available endpoints

    {
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/"
            }, 
            "categories": {
                "href": "http://api.guggenheim.org/calendar/categories/"
            }, 
            "events": {
                "href": "http://api.guggenheim.org/calendar/events/"
            }, 
            "instances": {
                "href": "http://api.guggenheim.org/calendar/instances/"
            }
        }
    }

Other endpoints may return similar lists of links in addition to specific resources.

## Events and Instances

One "event" may be repeated at different times, such as the showings of a film, so an Event resource consists of a title, description, category, etc. and a list of Instances. Every event must be associated with at least one instance. When an Instance resource is retrieved by itself, it will include all the descriptive fields of and a link to its parent Event.

## Instances

### Endpoints

<table>
    <thead>
        <tr>
            <th>Endpoint</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>/calendar/instances</td>
            <td>List of Instances on the current day</td>
        </tr>
        <tr>
            <td>calendar/instances/{id}</td>
            <td>One Instance</td>
        </tr>
        <tr>
            <td>calendar/instances/{YYYY-MM-DD}</td>
            <td>List of Instances on the specified day</td>
        </tr>
    </tbody>
</table>

### instances/

Retreive a list of instances for the current day and links to other endpoints.

#### Example Response

Request:

    http://api.guggenheim.org/calendar/instances/

Response:

    {
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/instances/"
            }, 
            "day": {
                "href": "http://api.guggenheim.org/calendar/instances/{YYYY-MM-DD}"
            }, 
            "item": {
                "href": "http://api.guggenheim.org/calendar/instances/{id}"
            }, 
            "month": {
                "href": "http://api.guggenheim.org/calendar/instances/{YYYY-MM}"
            }
        }, 
        "instances": [
            {
                "_links": {
                    "_self": {
                        "href": "http://api.guggenheim.org/calendar/instances/13444"
                    }, 
                    "event": {
                        "href": "http://api.guggenheim.org/calendar/events/815"
                    }, 
                    "web": {
                        "href": "http://www.guggenheim.org/new-york/calendar-and-events/2012/11/16/conservators-eye/i/13444"
                    }
                }, 
                "categories": [
                    {
                        "_links": {
                            "_self": {
                                "href": "http://api.guggenheim.org/calendar/categories/20"
                            }
                        }, 
                        "id": "20", 
                        "titles": "Tours & Gallery Programs"
                    }
                ], 
                "descriptions": {
                    "en": "Join Esther Chao, Associate Conservator, Objects, for a tour of <em>Gabriel Orozco: Asterisms</em>."
                }, 
                "id": "13444", 
                "start_date": "2012-11-16", 
                "start_time": "14:00:00", 
                "titles": {
                    "en": "Conservator's Eye"
                }
            }, 
            {
                "_links": {
                    "_self": {
                        "href": "http://api.guggenheim.org/calendar/instances/13443"
                    }, 
                    "event": {
                        "href": "http://api.guggenheim.org/calendar/events/812"
                    }, 
                    "web": {
                        "href": "http://www.guggenheim.org/new-york/calendar-and-events/2012/11/16/curators-eye-picasso-black-and-white/i/13443"
                    }
                }, 
                "categories": [
                    {
                        "_links": {
                            "_self": {
                                "href": "http://api.guggenheim.org/calendar/categories/20"
                            }
                        }, 
                        "id": "20", 
                        "titles": "Tours & Gallery Programs"
                    }, 
                    {
                        "_links": {
                            "_self": {
                                "href": "http://api.guggenheim.org/calendar/categories/69"
                            }
                        }, 
                        "id": "69", 
                        "titles": "Picasso Black and White"
                    }
                ], 
                "descriptions": {
                    "en": "Join Associate Curator Karole Vail for a tour of <em>Picasso Black and White</em>. Sign-up is required from noon to 1:45 pm at the Information Desk (the tour is limited to a maximum of 20 people)."
                }, 
                "id": "13443", 
                "start_date": "2012-11-16", 
                "start_time": "14:00:00", 
                "titles": {
                    "en": "Curator's Eye: Picasso Black and White"
                }
            }
        ], 
        "range": {
            "end_date": "2012-11-16", 
            "start_date": "2012-11-16"
        }
    }

### instances/{id}

Retrieve a single instance identified by ```id```

#### Example Response

Request:

    http://api.guggenheim.org/calendar/instances/13444

Response:

    {
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/instances/13444"
            }, 
            "day": {
                "href": "http://api.guggenheim.org/calendar/instances/{YYYY-MM-DD}"
            }, 
            "item": {
                "href": "http://api.guggenheim.org/calendar/instances/{id}"
            }, 
            "month": {
                "href": "http://api.guggenheim.org/calendar/instances/{YYYY-MM}"
            }
        }, 
        "instances": [
            {
                "_links": {
                    "_self": {
                        "href": "http://api.guggenheim.org/calendar/instances/13444"
                    }, 
                    "event": {
                        "href": "http://api.guggenheim.org/calendar/events/815"
                    }, 
                    "web": {
                        "href": "http://www.guggenheim.org/new-york/calendar-and-events/2012/11/16/conservators-eye/i/13444"
                    }
                }, 
                "categories": [
                    {
                        "_links": {
                            "_self": {
                                "href": "http://api.guggenheim.org/calendar/categories/20"
                            }
                        }, 
                        "id": "20", 
                        "titles": "Tours & Gallery Programs"
                    }
                ], 
                "descriptions": {
                    "en": "Join Esther Chao, Associate Conservator, Objects, for a tour of <em>Gabriel Orozco: Asterisms</em>."
                }, 
                "id": "13444", 
                "start_date": "2012-11-16", 
                "start_time": "14:00:00", 
                "titles": {
                    "en": "Conservator's Eye"
                }
            }
        ]
    }

### calendar/instances/{YYYY-MM-DD}, , calendar/instances/{YYYY-MM}

These endpoints return a list of instances for the given day (must be a valid 
date in YYYY-MM-DD format) or the current day if no date is specified. The 
simplest form ```calendar/instances``` is the equivalent of 
```/calendar/instances/{YYYY-MM-DD}?days=1``` where ```YYYY-MM-DD``` is the 
current date. ```calendar/instances/{YYYY-MM}``` returns a list of instances 
for the calendar month specified (that is, from the first to the last day)

<table>
    <thead>
        <th>Parameter</th>
        <th>Req'd?</th>
        <th>Description</th>
    </thead>
    <tbody>
        <tr>
            <td>days</td>
            <td>optional</td>
            <td>Number of days' worth or instances to retrieve (default 1, 
                maximum 31) inclusive of the start date. This parameter can
                also take one of two special values:

                <table>
                    <tr>
                        <th>week</th>
                        <td>Return instances for the week (Sun-Sat) in which
                            the day occurs. For instance, if the specified 
                            date where a Thursday, the response would 
                            include instances from the previous Sunday to the 
                            following Saturday (inclusive).</td>
                    </tr>
                    <tr>
                        <th>month</th>
                        <td>Return instances for the month in which the day 
                            occurs. For instance, if the specified date were
                            <code>2012-02-01</code>, with 
                            <code>days=month</code> the response would include instances from Feb. 1 to  Feb. 29 (inclusive). With 
                            <code>days=30</code> the  response would include 
                            instance from Feb. 1 to March 1.</td>
                    </tr>
                </table>
             </td>
        </tr>
    </tbody>
</table>


### Response Fields

<table>
    <thead>
        <tr>
            <th>Field</th>
            <th>Type</th>
            <th>Req'd?</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>start_date</td>
            <td>string</td>
            <td>REQUIRED</td>
            <td>Start date of the requested date range. Note: this MAY not be
                the date of the first instance returned.</td>
        </tr>
        <tr>
            <td>end_date</td>
            <td>string</td>
            <td>REQUIRED</td>
            <td>End date of the requested date range. Note: this MAY not be
                the date of the last instance returned.</td>
        </tr>
        <tr>
            <td>instances</td>
            <td>array</td>
            <td>REQUIRED</td>
            <td>An array of instance resources (see below).</td>
        </tr>
        <tr>
            <td>_links</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>A Links object. An Instances MUST contain a links to itself,
                (<code>_self</code>) to its parent Event (<code>event</code>), 
                and to its equivalent URL on the guggenheim.org website.
                (<code>web</code>)</td>
        </tr>

    </tbody>
</table>

### calendar/instances/{id}

Returns a single instance with the corresponding id.

### Example Response/Instance

    {
        "id": "13106", 
        "title": {
            "en": "Rineke Dijkstra Selects"
        }
        "start_date": "2012-08-31", 
        "start_time": "13:00:00", 
        "categories": [
            {
                "id": "22", 
                "title": "Film",
                "_links": {
                    "_self": {
                        "href": "http://dev0.guggenheim.org/calendar/categories/22"
                    }
                }
            }
        ], 
        "descriptions": {
            "en": "<strong><em>Blind Kind</em>, 1964</strong> Dir. Johan van der Keuken, 25 minutes, 35 mm Courtesy Mrs. N van der Lely and EYE Film Instituut Nederland <strong><em>Blanche-Neige Lucie</em>, 1997</strong> Pierre Huyghe, 4 minutes, DVD Courtesy Marian Goodman Gallery and Pierre Huyghe <strong><em>Fiorucci Made Me Hardcore</em>, 1999</strong> Mark Leckey, 15 minutes, DVD On the occasion of <em>Rineke Dijkstra: A Retrospective</em>, the Guggenheim presents a short program of film and video works carefully assembled by the artist. The program includes <em>Fiorucci Made Me Hardcore</em>, Mark Leckey's acclaimed short film portraying British nightlife from fragments of found video footage; Pierre Huyghe's <em>Blanche-Neige Lucie</em>, his short film capturing the struggle of Lucie Dolene, who sued Disney to regain possession of the copyright to her own voice in the French dubbed version of <em>Snow White and the Seven Dwarfs</em>; as well as <em>Blind Kind</em>, Johan van der Keuken's poetic short documentary about a school for blind children in Amsterdam, which captures firsthand the children's insight into their perception of the world around them."
        }, 
        "_links": {
            "_self": {
                "href": "http://dev0.guggenheim.org/calendar/instances/13106"
            }, 
            "event": {
                "href": "http://dev0.guggenheim.org/calendar/events/778"
            }, 
            "web": {
                "href": "http://www.guggenheim.org/new-york/calendar-and-events/2012/08/31/rineke-dijkstra-selects/i/13106"
            }
        } 
    }

### Instance fields

<table>
    <thead>
        <tr>
            <th>Field</th>
            <th>Type</th>
            <th>Req'd?</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>id</td>
            <td>number</td>
            <td>REQUIRED</td>
            <td>Unique ID number for the Instance</td>
        </tr>
        <tr>
            <td>titles</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>Internationalized titles for the event. Available titles are 
                listed by ISO-639-1 language code.</td>
        </tr>
        <tr>
            <td>descriptions</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>Internationalized descriptions for the event, also
                listed by ISO-639-1 language code. Descriptions may contain 
                HTML markup and should be handled accordingly.</td>
        </tr>
        <tr>
            <td>start_date</td>
            <td>string</td>
            <td>REQUIRED</td>
            <td>Start date of the instance in YYYY-MM-DD format</td>
        </tr>
        <tr>
            <td>start_time</td>
            <td>string</td>
            <td>REQUIRED</td>
            <td>Start time of the instance in HH:MM format using a 24-hour
                clock.</td>
        </tr>
        <tr>
            <td>end_date</td>
            <td>string</td>
            <td>OPTIONAL</td>
            <td>End date of the instance in YYYY-MM-DD format. Currently no 
                instances have an end_date, but we may at any time add them to
                the API. After that this property will be REQUIRED.</td>
        </tr>
        <tr>
            <td>end_time</td>
            <td>string</td>
            <td>OPTIONAL</td>
            <td>End time of the instance in HH:MM format using a 24-hour
                clock. Currently no instances have an end_time, but we may at 
                any time add them to the API. After that this property will be 
                REQUIRED.</td>
        </tr>
        <tr>
            <td>categories</td>
            <td>array</td>
            <td>REQUIRED</td>
            <td>An array of category object</td>
        </tr>
        <tr>
            <td>_links</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>A Links object. An Instance MUST contain a links to itself,
                (<code>_self</code>) to its parent Event (<code>event</code>), 
                and to its equivalent URL on the guggenheim.org website.
                (<code>web</code>)</td>
        </tr>
    </tbody>
</table>
 
