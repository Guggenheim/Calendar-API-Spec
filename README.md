Calendar-API-Spec
=================

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt

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
            <td>calendar/events</td>
            <td>List of Events on the current day</td>
        </tr>
        <tr>
            <td>calendar/events/{YYYY-MM-DD}/</td>
            <td>List of Events on the specified day</td>
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
            <td>calendar/instances/{YYYY-MM-DD}/</td>
            <td>List of Instances on the specified day</td>
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

## Events and Instances

One "event" may be repeated at different times, such as the showings of a film, so an Event resource consists of a title, description, category, etc. and a list of Instances. Every event must be associated with at least one instance. When an Instance resource is retrieved by itself, it will include all the descriptive fields of and a link to its parent Event.

When users are looking at calendar data for a specific timeframe, they should be be shown Instances rather than Events (since Instances of the Event outside the timeframe wouldn't be of any immediate interest). When users are viewing calendar data thematically (say, by Category) they should be shown Events first.

## Instances

### Endpoints

<table>
    <thead>
        <th>Endpoint</th>
        <th>Description</th>
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
            <td>calendar/instances/{YYYY-MM-DD}/</td>
            <td>List of Instances on the specified day</td>
        </tr>
    </tbody>
</table>

### calendar/instances, calendar/instances/{YYYY-MM-DD}

These endpoints return a list of instances for the given day (must be a valid 
date in YYYY-MM-DD format) or the current day if no date is specified. The 
simplest form ```calendar/instances``` is the equivalent of 
```/calendar/instances/{YYYY-MM-DD}?days=1``` where ```YYYY-MM-DD``` is the 
current date.

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
                            ```2012-02-01```, with ```days=month``` the 
                            response would include instances from Feb. 1 to 
                            Feb. 29 (inclusive). With ```days=30``` the 
                            response would include instance from Feb. 1 to 
                            March 1.</td>
                    </tr>
                </table>
             </td>
        </tr>
    </tbody>
</table>

### calendar/{id}

Returns a single instance with the corresponding id.

### Example Instance

    {
        "id": 13106,
        "titles": {
            "en": "Rineke Dijkstra Selects"
        },
        "descriptions": {
            "en": "<em>Blind Kind</em>, 1964. Courtesy EYE Film Instituut Nederland <strong><em>Blind Kind</em>, 1964</strong><br /> Dir. Johan van der Keuken, 25 minutes, 35 mm<br /> Courtesy Mrs. N van der Lely and EYE Film Instituut Nederland <strong><em>Blanche-Neige Lucie</em>, 1997</strong><br /> Pierre Huyghe, 4 minutes, DVD<br /> Courtesy Marian Goodman Gallery and Pierre Huyghe <strong><em>Fiorucci Made Me Hardcore</em>, 1999</strong><br /> Mark Leckey, 15 minutes, DVD On the occasion of <em>Rineke Dijkstra: A Retrospective</em>, the Guggenheim presents a short program of film and video works carefully assembled by the artist. The program includes <em>Fiorucci Made Me Hardcore</em>, Mark Leckey's acclaimed short film portraying British nightlife from fragments of found video footage; Pierre Huyghe's <em>Blanche-Neige Lucie</em>, his short film capturing the struggle of Lucie Dolene, who sued Disney to regain possession of the copyright to her own voice in the French dubbed version of <em>Snow White and the Seven Dwarfs</em>; as well as <em>Blind Kind</em>, Johan van der Keuken's poetic short documentary about a school for blind children in Amsterdam, which captures firsthand the children's insight into their perception of the world around them."
        }, 
        "start_date": "2012-08-31", 
        "start_time": "13:00:00", 
        "end_date": "2012-08-31", 
        "end_time": "14:00:00", 
        "category": {
            "id": "22", 
            "title": "Film",
            "_links": {
                "_self": "http://api.guggenheim.org/calendar/categories/22"
            } 
        }
        "_links": {
            "_self": {
                "href": "http://api.guggenheim.org/calendar/instances/13106"
            }, 
            "event": {
                "href": "http://api.guggenheim.org/calendar/events/778"
            },
            "web": {
                "href": "http://www.guggenheim.org/new-york/calendar-and-events/2012/08/31/rineke-dijkstra-selects/i/13106"
            }
        }, 
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
            <td>REQUIRED</td>
            <td>End date of the instance in YYYY-MM-DD format</td>
        </tr>
        <tr>
            <td>end_time</td>
            <td>string</td>
            <td>REQUIRED</td>
            <td>End time of the instance in HH:MM format using a 24-hour
                clock.</td>
        </tr>
        <tr>
            <td>category</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>A Category object</td>
        </tr>
        <tr>
            <td>_links</td>
            <td>object</td>
            <td>REQUIRED</td>
            <td>A Links object. An Instances MUST contain a links to itself,
            to its parent Event, and to its equivalent URL on the 
            guggenheim.org website.</td>
        </tr>
    </tbody>
</table>
 
