# DataSeed

An open platform for data visualisation, exploration and analysis. For more information and a live demo see [dataseed.co.uk](http://dataseed.co.uk).

## Front-End API

The front-end API uses [JSON](http://www.json.org/) as the data-interchange format. It has been desiged to minimise the amount of data transferred meaning it is not strictly [RESTful](http://en.wikipedia.org/wiki/Representational_state_transfer) although the concepts have been used where appropriate.

### Dataset

**Request**

```
GET /api/datasets/mortality
```

**Response**

```javascript
{
    "id": "mortality",
    "label": "Mortality in England and Wales",
    "measure": {
        "property": "value",
        "label": "Number of deaths"
    },
    "dimensions": [
        {
            "id": "gender",
            "label": "Gender",
            "type": ["bar"],
            "width": 3,
            "status": {
                "weight": 1,
                "format": "<strong>$s</strong>",
                "default_text": "people"
            }
        },
        {
            "id": "year",
            "label": "Year",
            "type": ["bar"],
            "width": 3,
            "status": {
                "weight": 4,
                "format": "in <strong>$</strong>",
                "default_text": "between 2002 and 2009"
            }
        }
    ]
}
```

### Dimension

**Request**

```
GET /api/datasets/dataset1/dimensions/gender
```

**Response**

```javascript
{
    "gender": {
        "F": {
            "id": "F",
            "label": "Female"
        },
        "M": {
            "id": "M",
            "label": "Male"
        }
    }
}
```

### Dimension Cut

```
GET /api/datasets/dataset1/dimensions/gender/query
```

When called without any parameters the total for each dimension value is returned. To return a "cut" version GET parameters should be passed where the key is the dimension ID and the value is the dimension value to cut on. For example, to select dimensions values for only the female gender in the year 2008 the request would be:

```
GET /api/datasets/dataset1/dimensions/gender/query?gender=F&year=2008
```

**Response**

```javascript
{
    "gender": [
        {
            "id": "F",
            "mean": 37.69967589656873,
            "max": 1867.0,
            "total": 2140286.0,
            "min": 0.0
        },
        {
            "id": "M",
            "mean": 36.26641752193961,
            "max": 1836.0,
            "total": 1950553.0,
            "min": 0.0
        }
    ]
}
```

## Import API

The import API also uses [JSON](http://www.json.org/) as the data-interchange format and is designed to be [RESTful](http://en.wikipedia.org/wiki/Representational_state_transfer). Authentication is performed using [HTTP Basic Access Authentication](http://tools.ietf.org/html/rfc2617) and all communication with the API is over [HTTPS](http://en.wikipedia.org/wiki/HTTP_Secure). 

### Create Dataset

**Request**

```
POST /api/datasets
```

```javascript
{
    "id": "dataset1",
    "label": "My Dataset",
    "measure": {
        "property": "value",
        "label": "Number of deaths"
    },
    "dimensions": [
        {
            "id": "gender",
            "label": "Gender",
            "type": ["bar"],
            "width": 3,
            "status": {
                "weight": 1,
                "format": "<strong>$s</strong>",
                "default_text": "people"
            }
        },
        {
            "id": "year",
            "label": "Year",
            "type": ["bar"],
            "width": 3,
            "status": {
                "weight": 4,
                "format": "in <strong>$</strong>",
                "default_text": "between 2002 and 2009"
            }
        }
    ]
}
```

**Response**

```javascript
{
    "status": 201,
    "message": "Created",
    "href": "/api/datasets/dataset1"
}
```

### Add Dimension Values

**Request**

```
PUT /api/datasets/dataset1/dimensions/gender
```

```
POST /api/datasets/dataset1/dimensions/gender
```

```javascript
{
    "values": [
        {
            "id": "M",
            "label": "Males",
            "short_label": "Men"
        },
        {
            "id": "F",
            "label": "Females",
            "short_label": "Women" 
        }
    ]
}
```

**Response**

```javascript
{
    "status": 201,
    "message": "Created",
    "href": "/api/datasets/dataset1/dimensions/gender"
}
```

### Add Observations

**Request**

```
PUT /api/datasets/dataset1/observations
```

```
POST /api/datasets/dataset1/observations
```

```javascript
{
    "observations": [
        {
            "gender": "M",
            "year": 2008,
            "value": 12
        },
        {
            "gender": "F",
            "year": 2008,
            "value": 18
        },
        {
            "gender": "M",
            "year": 2009,
            "value": 4
        },
        // ...
    ]
}
```

**Response**

```javascript
{
    "status": 201,
    "message": "Created",
    "href": "/api/datasets/dataset1/observations"
}
```
