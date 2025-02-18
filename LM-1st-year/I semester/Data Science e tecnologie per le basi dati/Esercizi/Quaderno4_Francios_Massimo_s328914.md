# Quaderno 4, Francios Massimo, s328914

### Domanda 1
```
db.bike_stations.aggregate(
	[{
	$match : {
		"extra.status": { $in : ["online", "offline"] }
	 }},{
	$group: {
		_id: "$extra.status",
		count: { $sum : 1}
	}}])
```
[ { _id: 'offline', count: 28 }, { _id: 'online', count: 33 } ]

### Domanda 2
```
db.bike_stations.aggregate(
	[{
	$match : {
		"extra.status": { $nin : ["online", "offline"] }
	 }},{
	$group: {
		_id: "$extra.status",
		count: { $sum : 1}
	}}])
```
\[ { _id: 'maintenance', count: 4 } \]

### Domanda 3
```
db.bike_stations.find({
	"extra.status": {$nin: ["online", "offline"]}
	},{
	"_id": 0, "extra.status": 1
	})
```
\[ { extra: { status: 'maintenance' } },
  { extra: { status: 'maintenance' } },
  { extra: { status: 'maintenance' } },
  { extra: { status: 'maintenance' } }
  \]

### Domanda 4
```
db.bike_stations.find(
	{"extra.status": "online",
	  "extra.score": {$gte: 4}},
	{"_id": 0, "name": 1}
).sort({"name": 1})
```
\[
  { name: '02. Pettiti' },
  { name: '04. Reggia' },
  { name: '06. Municipio' },
  { name: '08. San Marchese' },
  { name: '10. Gallo Praile' },
  { name: 'Belfiore' },
  { name: 'Borromini' },
  { name: 'Castello 1' },
  { name: 'Corte d'Appello' },
  { name: 'Giolitti 1' },
  { name: 'Politecnico 1' },
  { name: 'Politecnico 3' },
  { name: 'Porta Palatina' },
  { name: 'Principi d'Acaja 1' },
  { name: 'Principi d'Acaja 2' },
  { name: 'San Francesco da Paola' },
  { name: 'Sant´Anselmo' },
  { name: 'Tribunale' }
\]

### Domanda 5
```
db.bike_stations.find(
	{"extra.status": "offline",
	$or : [
	 { "empty_slots": { $gt: 0 } },
     { "free_bikes": { $gt: 0 } }
	]
	},
	{"_id":0, "name": 1, "empty_slots": 1, "free_bikes": 1}
)
```
 { empty_slots: 1, free_bikes: 0, name: '06. Le Serre' },
 { empty_slots: 0, free_bikes: 5, name: '05. Corso Garibaldi' }

### Domanda 6
```
db.bike_stations.aggregate([
  {$group: {
      _id: null,
      totalReviews: { $sum: "$extra.reviews" }
    }}])
```
{ _id: null, totalReviews: 15311 } 

### Domanda 7
```
db.bike_stations.aggregate([
  {$group: {
      _id: "$extra.score",
      count: { $sum: 1 }
    }},
  {$sort: {
      _id: -1
    }}])
```
  { _id: 4.7, count: 1 }, { _id: 4.5, count: 2 },
  { _id: 4.4, count: 2 }, { _id: 4.3, count: 2 },
  { _id: 4.2, count: 7 }, { _id: 4.1, count: 5 },
  { _id: 4, count: 9 },   { _id: 3.9, count: 9 },
  { _id: 3.8, count: 1 }, { _id: 3.7, count: 2 },
  { _id: 3.6, count: 1 }, { _id: 3.5, count: 4 },
  { _id: 3.4, count: 3 }, { _id: 3.2, count: 1 },
  { _id: 3, count: 4 },   { _id: 2.8, count: 2 },
  { _id: 2.7, count: 1 }, { _id: 2.5, count: 1 },
  { _id: 2.4, count: 1 }, { _id: 2.1, count: 1 }

### Domanda 8
```
db.bike_stations.aggregate([
	{$match: { "extra.status" : {$in : ["online", "offline"]}}},
	{$group: {
	_id: "$extra.status",
	valutazioneMedia : { $avg : "$extra.score"}
	}}
])
```
  { _id: 'online', valutazioneMedia: 3.8454545454545452 },
  { _id: 'offline', valutazioneMedia: 3.0285714285714285 }

### Domanda 9
```
db.bike_stations.aggregate([
  {
    $bucket: {
      groupBy: "$free_bikes",
      boundaries: [0, 1, Infinity],
      default: "default", 
      output: {
        averageScore: { $avg: "$extra.score" }
      }
    }
  }
])

```
  { _id: 0, averageScore: 3.2305555555555556 },
  { _id: 1, averageScore: 3.8758620689655174 }
### Domanda 10
```
db.bike_stations.aggregate([
  {
    $match: { "extra.status": "online" }
  },
  {
    $bucket: {
      groupBy: "$free_bikes",
      boundaries: [0, 1, Infinity],
      default: "default",
      output: {
        averageScore: { $avg: "$extra.score" }
      }
    }
  }
])

```
{ _id: 0,  averageScore: 3.7399999999999998 }
{ _id: 1,  averageScore: 3.8642857142857143}
### Domanda 11
```
db.bike_stations.find(
  {
    "free_bikes": { $gt: 0 },
    "location": {
      $near: {
        $geometry: {
          type: "Point",
          coordinates: [45.07456, 7.69463]
    }}}
  },
  {"name": 1, "free_bikes":1, "_id": 0}
).limit(3)
```
  { free_bikes: 5, name: 'Palermo 2' },
  { free_bikes: 5, name: 'Castello 1' },
  { free_bikes: 4, name: 'San Francesco da Paola' }

### Domanda 12
```
db.bike_stations.find({
  free_bikes: {$gt: 0},
  location: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: db.bike_stations.findOne({name: "Politecnico 4"}).location.coordinates
      }
    }
  }
}, {_id:0, name:1, free_bikes:1}).limit(3)

```
  { free_bikes: 9, name: 'Politecnico 1' },
  { free_bikes: 5, name: 'Politecnico 3' },
  { free_bikes: 3, name: 'Tribunale' }
 