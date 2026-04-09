# Sink/Gateway Edge Node

Receive data from all edge nodes, store data, visualise system state and allow user configuration.

## Producers 
The gateway is also the single source of truth and does not require other nodes to keep track of any one thing, simply that it notifies the gateway of a change.

### Occupancy
Notify how many passengers have increased/decreased

```json
{
  "train_id": "<train id>",
  "occupancy_change": "positive number to indicate increase and negative to indicate decrease",
}
```

### Train (arrival)
Indicate that a train has arrived at a station.

```json
{
  "train_id": "<train id>",
  "train_arriving": "true for arriving, false for departing",
  "station_id" "<station id>"
}
```

## Consumers
Consumers should not directly communicate with producers and should go through the gateway node.

### Station 1 and Station 2
Station 1 and Station 2 will want to be subscribed to hear about train arrivals and departures, as well as occupancy levels.

```json
{
  "train_id": "<train id>",
  "train_arriving": "true for arriving, false for departing",
  "station_id" "<station id>"
}
```

## Database Architecture
The gateway stores occupancy levels and logs of train arrivals and departures. 

### stations
| Col    | Type |
| -------- | ------- |
| **id**  | integer unique automatically generated identifier    |
| **name** | string unique station name     |
| **created_at** | datetime log was created at     |
| **updated_at** | datetime log was updated at     |

### trains
| Col    | Type |
| -------- | ------- |
| **id**  | integer unique automatically generated identifier    |
| **name** | string unique train name     |
| **created_at** | datetime log was created at     |
| **updated_at** | datetime log was updated at     |

### train_logs
| Col    | Type |
| -------- | ------- |
| **id**  | integer unique automatically generated identifier    |
| **log** | json log     |
| **created_at** | datetime log was created at     |
| **updated_at** | datetime log was updated at     |

To support a wide range of logs, the log itself will be made of json, taking inspiration from the structure of [Spatie Laravel Activity Log](https://spatie.be/docs/laravel-activitylog/v5/introduction) package. This allows for a single table of logs. This package is typically used for keeping track of table changes, however, it can have custom logs in the form of:

```
[
    'attributes' => [
        'name' => 'updated name',
        'text' => 'Lorem',
    ],
    'old' => [
        'name' => 'original name',
        'text' => 'Lorem',
    ],
];
```

From previous experience the best way to have custom logs is to include an identifier as to the type of log being kept, and when iterating through for displaying or analysing, it can be passed on to a handler for that specific type. For example:

```
[
    'type' => 'station_change',
    .
    .
    .
];
```


```
[
    'type' => 'occupancy_change',
    .
    .
    .
];
```
