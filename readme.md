# hafas-find-trips

**Provide location and bearing/heading, get the public transport vehicle you're most likely in.**

Location and bearing/heading are expected to be inaccurate because they come from a mobile device. Also, The vehicle movements from [the underlying `radar()` API](https://github.com/public-transport/hafas-client/blob/ecc26ef313b75f9bedcf4ee1b2b95aebb3478379/docs/trip.md) are often *not* the actual position, but the estimated position, based on their current delays and their track. To compensate for this, `hafas-find-trips`

- filters by product if you provide one,
- checks if the location is close to where vehicles have recently been or will soon be,
- takes the bearing/heading of each vehicle into account.

[![npm version](https://img.shields.io/npm/v/hafas-find-trips.svg)](https://www.npmjs.com/package/hafas-find-trips)
![ISC-licensed](https://img.shields.io/github/license/derhuerst/hafas-find-trips.svg)
[![chat with me on Gitter](https://img.shields.io/badge/chat%20with%20me-on%20gitter-512e92.svg)](https://gitter.im/derhuerst)
[![support me on Patreon](https://img.shields.io/badge/support%20me-on%20patreon-fa7664.svg)](https://patreon.com/derhuerst)


## Installation

```shell
npm install hafas-find-trips
```


## Usage

Provide a track of user locations from the last ~10 seconds as [GeoJSON `LineString`](https://tools.ietf.org/html/rfc7946#section-3.1.4). If possible, provide a product in addition.

```js
const findTrips = require('hafas-find-trips')
const createHafas = require('vbb-hafas')

const hafas = createHafas('my-awesome-program')

findTrips(hafas, {
	recording: {
		type: 'Feature',
		properties: {},
		geometry: { // GeoJSON LineString
			type: 'LineString',
			coordinates: [ /* … */ ]
		}
	},
	product: 'subway' // optional
})
.then((matches) => {
	for (let match of matches) {
		const m = match.movement
		console.log(match.score, m.line.name, m.direction, m.location)
	}
})
.catch((err) => {
	console.error(err)
	process.exitCode = 1
})
```

If you don't provide the product, it will instead apply its heuristic to all vehicles nearby.


## Contributing

If you have a question or have difficulties using `hafas-find-trips`, please double-check your code and setup first. If you think you have found a bug or want to propose a feature, refer to [the issues page](https://github.com/derhuerst/hafas-find-trips/issues).
