<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>Create a hover effect</title>
<meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
<script src="https://api.mapbox.com/mapbox-gl-js/v1.10.1/mapbox-gl.js"></script>
<link href="https://api.mapbox.com/mapbox-gl-js/v1.10.1/mapbox-gl.css" rel="stylesheet" />
<style>
	body { margin: 0; padding: 0; }
	#map { position: absolute; top: 0; bottom: 0; width: 100%; }
</style>
</head>
<body>
<div id="map"></div>
<script>
	mapboxgl.accessToken = 'pk.eyJ1IjoibWFwc2J5bW9uIiwiYSI6ImNrNmoyMXE0MjA2MnQzZXQwZTZjbXB2OGcifQ.8xFr7oUTNOPpvFYXHgCJTA';
var map = new mapboxgl.Map({
container: 'map',
style: 'mapbox://styles/mapbox/streets-v11',
center: [-100.486052, 37.830348],
zoom: 2
});
var hoveredStateId = null;
 
map.on('load', function() {
map.addSource('states', {
'type': 'geojson',
'data':
'https://docs.mapbox.com/mapbox-gl-js/assets/us_states.geojson'
});
 
// The feature-state dependent fill-opacity expression will render the hover effect
// when a feature's hover state is set to true.
map.addLayer({
'id': 'state-fills',
'type': 'fill',
'source': 'states',
'layout': {},
'paint': {
'fill-color': '#627BC1',
'fill-opacity': [
'case',
['boolean', ['feature-state', 'hover'], false],
1,
0.5
]
}
});
 
map.addLayer({
'id': 'state-borders',
'type': 'line',
'source': 'states',
'layout': {},
'paint': {
'line-color': '#627BC1',
'line-width': 2
}
});
 
// When the user moves their mouse over the state-fill layer, we'll update the
// feature state for the feature under the mouse.
map.on('mousemove', 'state-fills', function(e) {
if (e.features.length > 0) {
if (hoveredStateId) {
map.setFeatureState(
{ source: 'states', id: hoveredStateId },
{ hover: false }
);
}
hoveredStateId = e.features[0].id;
map.setFeatureState(
{ source: 'states', id: hoveredStateId },
{ hover: true }
);
}
});
 
// When the mouse leaves the state-fill layer, update the feature state of the
// previously hovered feature.
map.on('mouseleave', 'state-fills', function() {
if (hoveredStateId) {
map.setFeatureState(
{ source: 'states', id: hoveredStateId },
{ hover: false }
);
}
hoveredStateId = null;
});
});
</script>
 
</body>
</html>
