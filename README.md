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
            <td>/</td>
            <td>Index of available resources</td>
        </tr>
        <tr>
            <td><a href="#instances-1">/instances</a></td>
            <td>List of Instances on the current day</td>
        </tr>
        <tr>
            <td><a href="#instancesid">/instances/{id}</a></td>
            <td>One Instance</td>
        </tr>
        <tr>
            <td><a href="#instancesyyyy-mm-dd">/instances/{YYYY-MM-DD}</a></td>
            <td>List of Instances on the specified day</td>
        </tr>
        <tr>
            <td><a href="#events-1">/events</a></td>
            <td>Index of available event resources</td>
        </tr>
        <tr>
            <td><a href="#eventsid">/events/{id}</a></td>
            <td>One Event</td>
        </tr>
        <tr>
            <td>/categories</td>
            <td>List of Categories</td>
        </tr>
        <tr>
            <td>/categories/{id}</td>
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
            <td>/instances</td>
            <td>List of Instances on the current day</td>
        </tr>
        <tr>
            <td><a href="#instancesid">/instances/{id}</a></td>
            <td>One Instance</td>
        </tr>
        <tr>
            <td><a href="#instancesyyyy-mm-dd">/instances/{YYYY-MM-DD}</a></td>
            <td>List of Instances on the specified day</td>
        </tr>
    </tbody>
</table>

### instances/

Retrieve a list of instances for the current day and links to other endpoints.

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

#### Response Fields

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
            <td>instances</td>
            <td>array</td>
            <td>REQUIRED</td>
            <td>An array of instance resources (see below).</td>
        </tr>
        <tr>
            <td>range</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>An object with two properties, <code>start_date</code> and <code>end_date</code> (both "YYYY-MM-DD" date strings), indicating the date range covered by the request.</td>
        </tr>
        <tr>
            <td>_links</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>A Links object.</td>
        </tr>

    </tbody>
</table>


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

#### Response Fields

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
            <td>instances</td>
            <td>array</td>
            <td>REQUIRED</td>
            <td>An array of instance resources (see below). This endpoint currently always returns an array containing one item, but in the future it may accept a list of instance ids to retrieve.</td>
        </tr>
        <tr>
            <td>_links</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>A Links object.</td>
        </tr>

    </tbody>
</table>


### instances/{YYYY-MM-DD}

Retrieve a list of instances for the given date or date range. The range can be specified with the ```days``` parameter which can take the follwing values

<table>
    <thead>
        <th>Value</th>
        <th>Example</th>
        <th>Result</th>
    </thead>
    <tbody>
        <tr>
            <td>An integer</td>
            <td><code>instances/2012-03-15?days=7</code></td>
            <td>The number of days following the given date, inclusive. This example would retrieve all instances from March 15, 2012 to March 21, 2012.</td>
        </tr>
        <tr>
            <td>'week'</td>
            <td><code>instances/2012-03-15?days=week</code></td>
            <td>The week (Sunday-Saturday) containing the date. This example would retrieve all instances from March 11, 2012 to March 17, 2012. <code>instances/2012-03-14?days=week</code> would return the same result.</td>
        </tr>
        <tr>
            <td>'month'</td>
            <td><code>instances/2012-03-15?days=month</code></td>
            <td>The calendar month containing the date. This example would retrieve all instances from March 1, 2012 to March 31, 2012</td>
        </tr>
    </tbody>
</table>

Response fields are the same as ```instances/```.

## Events

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
            <td><a href="#events-1">/events</a></td>
            <td>Index of available event resources</td>
        </tr>
        <tr>
            <td><a href="#eventsid">/events/{id}</a></td>
            <td>One Event</td>
        </tr>
    </tbody>
</table>

### events/

Retrieve a list of links to other event resources.

#### Example Response

Request:

    http://api.guggenheim.org/calendar/events/

Response:

    {
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/events/"
            }, 
            "item": {
                "href": "http://api.guggenheim.org/calendar/events/{id}"
            }
        }
    }

### events/{id}

Retrieve a single event

#### Example Response

Request:

    http://api.guggenheim.org/calendar/events/815

Response:

    {
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/events/815"
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
        "instances": [
            {
                "_links": {
                    "_self": {
                        "href": "http://api.guggenheim.org/calendar/instances/13444"
                    }, 
                    "web": {
                        "href": "http://www.guggenheim.org/new-york/calendar-and-events/2012/11/16/conservators-eye/i/13444"
                    }
                }, 
                "id": "13444", 
                "start_date": "2012-11-16", 
                "start_time": "14:00:00"
            }
        ], 
        "titles": {
            "en": "Conservator's Eye"
        }
    }

See [Event Object Fields](#event-object-fields)

## Categories

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
            <td><a href="#categories-1">/categories</a></td>
            <td>Index of available event resources</td>
        </tr>
        <tr>
            <td><a href="#categoriesid">categories/{id}</a></td>
            <td>One Event</td>
        </tr>
    </tbody>
</table>

### categories/

Retrieve a list of categories and links to other category resources.

#### Example Response

Request:

    http://api.guggenheim.org/calendar/categories/

Response:

    {
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/categories/"
            }, 
            "item": {
                "href": "http://api.guggenheim.org/calendar/categories/{id}"
            }
        }, 
        "categories": [
            {
                "_links": {
                    "_self": {
                        "href": "http://api.guggenheim.org/calendar/categories/5"
                    }, 
                    "parent": {
                        "_links": {
                            "item": {
                                "href": "http://api.guggenheim.org/calendar/categories/1"
                            }
                        }, 
                        "category": {
                            "en": "Audience"
                        }
                    }
                }, 
                "category": {
                    "en": "Adults"
                }, 
                "id": "5"
            }, 
            {
                "_links": {
                    "_self": {
                        "href": "http://api.guggenheim.org/calendar/categories/6"
                    }, 
                    "parent": {
                        "_links": {
                            "item": {
                                "href": "http://api.guggenheim.org/calendar/categories/1"
                            }
                        }, 
                        "category": {
                            "en": "Audience"
                        }
                    }
                }, 
                "category": {
                    "en": "Educators"
                }, 
                "id": "6"
            }, 
            {...},
            {...},
            ...
        ]
    }
### categories/{id}

Retrieve a single category.

#### Example Response

Request:

    http://api.guggenheim.org/calendar/categories/20

Response:

    {
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/categories/20"
            }, 
            "parent": {
                "_links": {
                    "item": {
                        "href": "http://api.guggenheim.org/calendar/categories/2"
                    }
                }, 
                "category": {
                    "en": "Event"
                }
            }
        }, 
        "category": {
            "en": "Tours & Gallery Programs"
        }, 
        "id": "20", 
        "instances": [
            {
                "_links": {
                    "_self": {
                        "href": "http://api.guggenheim.org/calendar/instances/13445"
                    }, 
                    "event": {
                        "href": "http://api.guggenheim.org/calendar/events/816"
                    }, 
                    "web": {
                        "href": "http://www.guggenheim.org/new-york/calendar-and-events/2012/11/30/curators-eye-picasso-black-and-white/i/13445"
                    }
                }, 
                "descriptions": {
                    "en": "Join Joan Young, Director, Curatorial Affairs, for a tour of <em>Gabriel Orozco: Asterisms</em>. This Curator's Eye Tour will be ASL-interpreted."
                }, 
                "id": "13445", 
                "start_date": "2012-11-30", 
                "start_time": "14:00:00", 
                "titles": {
                    "en": "Curator's Eye"
                }
            }
        ], 
        "range": {
            "end_date": "2012-11-30", 
            "start_date": "2012-11-19"
        }
    }

### Instance Object Fields

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
            <td>An array of category objects</td>
        </tr>
        <tr>
            <td>_links</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>A Links object. An Instance MUST contain a links to itself
                (<code>_self</code>), to its parent Event 
                (<code>event</code>), and to its equivalent URL on the guggenheim.org website (<code>web</code>).</td>
        </tr>
    </tbody>
</table>

### Event Object Fields

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
            <td>Unique ID number for the Event</td>
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
            <td>categories</td>
            <td>array</td>
            <td>REQUIRED</td>
            <td>An array of category objects</td>
        </tr>
        <tr>
            <td>instances</td>
            <td>array</td>
            <td>REQUIRED</td>
            <td>An array of instance objects</td>
        </tr>
        <tr>
            <td>_links</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>A Links object. An Instance MUST contain a links to itself
                (<code>_self</code>), to its parent Event 
                (<code>event</code>), and to its equivalent URL on the guggenheim.org website (<code>web</code>).</td>
        </tr>
    </tbody>
</table>
 
