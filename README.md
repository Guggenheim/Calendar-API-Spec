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
            <td><a href="#categories-1">/categories</a></td>
            <td>List of Categories</td>
        </tr>
        <tr>
            <td><a href="#categoriesid">/categories/{id}</a></td>
            <td>One Category</td>
        </tr>
        <tr>
            <td><a href="#second_categories-1">/second_categories</a></td>
            <td>List of Categories</td>
        </tr>
        <tr>
            <td><a href="#second_categoriesid">/second_categories/{id}</a></td>
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
            "events": {
                "href": "http://api.guggenheim.org/calendar/events/"
            }, 
            "instances": {
                "href": "http://api.guggenheim.org/calendar/instances/"
            },
            "categories": {
                "href": "http://api.guggenheim.org/calendar/categories/"
            }, 
            "second_categories": {
                "href": "http://api.guggenheim.org/calendar/second_categories/"
            }, 
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
            <td><a href="#instances">/instances</a></td>
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
                        "href": "http://api.guggenheim.org/calendar/instances/2392"
                    }, 
                    "event": {
                        "href": "http://api.guggenheim.org/calendar/events/831"
                    }, 
                    "rsvp": {
                        "href": "mailto:ycc@guggenheim.org"
                    }, 
                    "web": {
                        "href": "http://www.guggenheim.org/new-york/calendar-and-events/2013/09/12/opening-reception-supernatural-contemporary-korean-art-/2392"
                    }
                }, 
                "categories": [
                    {
                        "_links": {
                            "_self": {
                                "href": "http://api.guggenheim.org/calendar/categories/1"
                            }
                        }, 
                        "id": "1", 
                        "titles": "Tours & Gallery Programs"
                    }
                ], 
                "descriptions": {
                    "en": "Young Collectors Council members are invited to attend the opening reception for <em>Super/Natural Contemporary Korean Art</em>, a private sales exhibition at Christie\u2019s. In addition, Christie\u2019s can accommodate a small group of YCC members for an exclusive tour to preview two of Christie\u2019s upcoming auctions: <em>South Asian Modern + Contemporary Art</em> and <em>The Art of Nandalal Bose, Abanindranath Tagore and Rabindranath Tagore: The Supratik Bose Collection</em>. Please specify in your RSVP if you would like to attend the tour and reception or just the reception. Space is limited for the tour but not the reception."
                }, 
                "id": "2392", 
                "media": [
                    {
                        "assets": {
                            "thumbnail": {
                                "_links": {
                                    "_self": {
                                        "href": "http://media.guggenheim.org/content/New_York/events/stock/ycc-only.png"
                                    }
                                }, 
                                "height": 135, 
                                "width": 205
                            }
                        }, 
                        "format": "PNG", 
                        "orientation": "landscape", 
                        "type": "Image"
                    }
                ], 
                "sold_out": false, 
                "start_date": "2013-09-12", 
                "start_time": "18:00:00", 
                "titles": {
                    "en": "Opening Reception: Super/Natural Contemporary Korean Art"
                }
            }
        ], 
        "range": {
            "end_date": "2013-09-12", 
            "start_date": "2013-09-12"
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

    http://api.guggenheim.org/calendar/instances/1604

Response:

    {
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/instances/1604"
            }
        }, 
        "instances": [
            {
                "_links": {
                    "_self": {
                        "href": "http://api.guggenheim.org/calendar/instances/1604"
                    }, 
                    "event": {
                        "href": "http://api.guggenheim.org/calendar/events/792"
                    }, 
                    "web": {
                        "href": "http://www.guggenheim.org/new-york/calendar-and-events/2013/09/13/educators-eye-daily-public-tour/1604"
                    }
                }, 
                "categories": [
                    {
                        "_links": {
                            "_self": {
                                "href": "http://api.guggenheim.org/calendar/categories/1"
                            }
                        }, 
                        "id": "1", 
                        "titles": "Tours & Gallery Programs"
                    }
                ], 
                "descriptions": {
                    "en": "Daily tours are led by museum educators with diverse backgrounds in art history, art education, and artistic practice related to Modern and contemporary art. Educators are committed to providing informative and meaningful experiences by engaging visitors in a shared process of close looking and conversation. For everyone from first-time visitors to long-term members, daily tours are invaluable for learning about the collections, special exhibitions, and the Frank Lloyd Wright building. Visitors of all ages and abilities are encouraged and welcome to join. Daily tours are free with museum admission and offered at 11 am and 1 pm. Assistive listening devices are used on all daily tours."
                }, 
                "id": "1604", 
                "media": [
                    {
                        "assets": {
                            "thumbnail": {
                                "_links": {
                                    "_self": {
                                        "href": "http://media.guggenheim.org/content/New_York/events/stock/edseyetour.png"
                                    }
                                }, 
                                "height": 135, 
                                "width": 205
                            }
                        }, 
                        "format": "PNG", 
                        "orientation": "landscape", 
                        "type": "Image"
                    }
                ], 
                "second_categories": [
                    [
                        {
                            "_links": {
                                "_self": {
                                    "href": "http://api.guggenheim.org/calendar/second_categories/2"
                                }
                            }, 
                            "id": "2", 
                            "titles": "Free with Admission"
                        }
                    ]
                ], 
                "sold_out": false, 
                "start_date": "2013-09-13", 
                "start_time": "11:00:00", 
                "titles": {
                    "en": "Educator\u2019s Eye Daily Public Tour"
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
 
## Second Categories

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
            <td><a href="#second_categories-1">/second_categories</a></td>
            <td>Index of available event resources</td>
        </tr>
        <tr>
            <td><a href="#second_categoriesid">second_categories/{id}</a></td>
            <td>One Event</td>
        </tr>
    </tbody>
</table>

### second_categories/

Retrieve a list of second categories and links to other second category resources.

#### Example Response

Request:

    http://api.guggenheim.org/calendar/second_categories/

Response:

     {
    	"_links": {
    		"_self": {
    			"href": "http://api.guggenheim.org/calendar/second_categories/"
    		},
    		"item": {
    			"href": "http://api.guggenheim.org/calendar/second_categories/{id}"
    		}
    	},
    	"second_categories": [
    		{
    			"id": "8",
    			"category": {
    				"en": "For Educators"
    			},
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/second_categories/8"
    				}
    			}
    		},
    		{
    			"id": "6",
    			"category": {
    				"en": "For Kids"
    			},
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/second_categories/6"
    				}
    			}
    		},
    		{
    			"id": "5",
    			"category": {
    				"en": "For Members Only"
    			},
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/second_categories/5"
    				}
    			}
    		},
    		{
    			"id": "9",
    			"category": {
    				"en": "For Visitor with Disabilities"
    			},
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/second_categories/9"
    				}
    			}
    		},
    		{
    			"id": "2",
    			"category": {
    				"en": "Free with Admission"
    			},
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/second_categories/2"
    				}
    			}
    		},
    		{
    			"id": "24",
    			"category": {
    				"en": "Online Events"
    			},
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/second_categories/24"
    				}
    			}
    		},
    		{
    			"id": "7",
    			"category": {
    				"en": "Teens & Family"
    			},
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/second_categories/7"
    				}
    			}
    		}
    	]
    }

### second_categories/{id}

Retrieve a single second category.

#### Example Response

Request:

    http://api.guggenheim.org/calendar/second_categories/5

Response:

    {
    	"id": "5",
    	"category": {
    		"en": "For Members Only"
    	},
    	"_links": {
    		"_self": {
    			"href": "http://api.guggenheim.org/calendar/second_categories/5"
    		}
    	},
    	"range": {
    		"start_date": "2013-09-11",
    		"end_date": "2013-09-30"
    	},
    	"instances": [
    		{
    			"id": "1375",
    			"titles": {
    				"en": "Members\u2019 Morning Private View\u2014James Turrell"
    			},
    			"descriptions": {
    				"en": "Beat the crowds and join fellow members for a visit to the museum before it opens to the public. Enjoy a private viewing of the exhibition <em>James Turrell</em>. James Turrell\u2019s first exhibition in a New York museum since 1980 focuses on the artist\u2019s groundbreaking explorations of perception, light, color, and space, with a special focus on the role of site-specificity in his practice. At its core is a major new project that recasts the Guggenheim rotunda as an enormous volume filled with shifting artificial and natural light. One of the most dramatic transformations of the museum ever conceived, the installation reimagines Frank Lloyd Wright\u2019s iconic architecture\u2014its openness to nature, graceful curves, and magnificent sense of space\u2014as one of Turrell\u2019s <em>Skyspaces</em>, referencing in particular his magnum opus <em>Roden Crater</em> (1976\u2013 ). Become a member today and enjoy this unique opportunity to see the museum and attend many other exclusive events."
    			},
    			"start_date": "2013-09-15",
    			"start_time": "09:00:00",
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/instances/1375"
    				},
    				"event": {
    					"href": "http://api.guggenheim.org/calendar/events/737"
    				},
    				"web": {
    					"href": "http://www.guggenheim.org/new-york/calendar-and-events/2013/09/15/members-morning-private-viewjames-turrell/1375"
    				}
    			}
    		},
    		{
    			"id": "2376",
    			"titles": {
    				"en": "Private Tour of Music at the Aldrich"
    			},
    			"descriptions": {
    				"en": "Join the Aldrich Contemporary Art Museum\u2019s Exhibitions Director Richard Klein, Curator Amy Smith-Stewart, and curatorial consultant Kelly Taxter for a private tour of the exhibition <em>Music</em> on its opening day. The opening will feature the premiere of <em>Number 17</em>, an endurance-based performance by Xaviera Simmons. Food will be available for purchase from food trucks in the Sculpture Garden. Transportation will be provided by the Aldrich for a fee of $15 and will depart at 12:30 pm from MoMA PS1 in Long Island City, returning at 6 pm."
    			},
    			"start_date": "2013-09-22",
    			"start_time": "14:00:00",
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/instances/2376"
    				},
    				"event": {
    					"href": "http://api.guggenheim.org/calendar/events/819"
    				},
    				"web": {
    					"href": "http://www.guggenheim.org/new-york/calendar-and-events/2013/09/22/private-tour-of-music-at-the-aldrich/2376"
    				}
    			}
    		},
    		{
    			"id": "2280",
    			"titles": {
    				"en": "Members-Only Quiet Views of Aten Reign"
    			},
    			"descriptions": {
    				"en": "Given the popularity of the <em>James Turrell</em> exhibition as well as the intimate, meditative nature of Turrell\u2019s work, we are adding special opportunities for Guggenheim members to spend a quiet hour communing with <em>Aten Reign</em> during a new series of exclusive events."
    			},
    			"start_date": "2013-09-23",
    			"start_time": "19:30:00",
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/instances/2280"
    				},
    				"event": {
    					"href": "http://api.guggenheim.org/calendar/events/814"
    				},
    				"web": {
    					"href": "http://www.guggenheim.org/new-york/calendar-and-events/2013/09/23/members-only-quiet-view-of-james-turrell/2280"
    				}
    			}
    		},
    		{
    			"id": "2281",
    			"titles": {
    				"en": "Members-Only Quiet Views of Aten Reign"
    			},
    			"descriptions": {
    				"en": "Given the popularity of the <em>James Turrell</em> exhibition as well as the intimate, meditative nature of Turrell\u2019s work, we are adding special opportunities for Guggenheim members to spend a quiet hour communing with <em>Aten Reign</em> during a new series of exclusive events."
    			},
    			"start_date": "2013-09-23",
    			"start_time": "21:00:00",
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/instances/2281"
    				},
    				"event": {
    					"href": "http://api.guggenheim.org/calendar/events/814"
    				},
    				"web": {
    					"href": "http://www.guggenheim.org/new-york/calendar-and-events/2013/09/23/members-only-quiet-view-of-james-turrell/2281"
    				}
    			}
    		},
    		{
    			"id": "2377",
    			"titles": {
    				"en": "Artist Dinner with Julia Dault"
    			},
    			"descriptions": {
    				"en": "Join fellow Young Collectors Council members for an intimate dinner with artist Julia Dault, hosted by YCC cochair Astrid T. Hill. In 2012, the YCC Acquisitions Committee voted to acquire Julia Dault&rsquo;s painting <em>Heavy Metal</em> (2012). This exclusive event, open only to YCC members, includes a three-course dinner in the museum&rsquo;s James Beard Award&ndash;winning restaurant the Wright followed by a conversation between the artist and Associate Curator Katherine Brinson. Tickets are $125 per person; proceeds benefit the YCC Art Fund. Cocktail attire is suggested."
    			},
    			"start_date": "2013-09-24",
    			"start_time": "19:00:00",
    			"_links": {
    				"_self": {
    					"href": "http://api.guggenheim.org/calendar/instances/2377"
    				},
    				"event": {
    					"href": "http://api.guggenheim.org/calendar/events/820"
    				},
    				"web": {
    					"href": "http://www.guggenheim.org/new-york/calendar-and-events/2013/09/24/artist-dinner-with-julia-dault/2377"
    				}
    			}
    		}
    	]
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
 
