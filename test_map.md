---
layout: page
title: Test Map
permalink: /map/
robots: noindex, nofollow
---


<h3>Using GeoJSON with Leaflet</h3>

<p>GeoJSON is becoming a very popular data format among many GIS technologies and services - it's simple, lightweight, straightforward, and Leaflet is quite good at handling it. In this example, you'll learn how to create and interact with map vectors created from <a href="http://geojson.org/">GeoJSON</a> objects.</p>

<div id="map" class="map" style="height: 450px"></div>

<script src="examples/geojson/sample-geojson.js"></script>
<script>

	var map = L.map('map').setView([39.74739, -105], 13);

	L.tileLayer(MB_URL, {
		attribution: MB_ATTR,
		id: 'mapbox.light'
	}).addTo(map);

	var baseballIcon = L.icon({
		iconUrl: 'baseball-marker.png',
		iconSize: [32, 37],
		iconAnchor: [16, 37],
		popupAnchor: [0, -28]
	});

	function onEachFeature(feature, layer) {
		var popupContent = "<p>I started out as a GeoJSON " +
				feature.geometry.type + ", but now I'm a Leaflet vector!</p>";

		if (feature.properties && feature.properties.popupContent) {
			popupContent += feature.properties.popupContent;
		}

		layer.bindPopup(popupContent);
	}

	L.geoJson({features: [bicycleRental, campus]}, {

		style: function (feature) {
			return feature.properties && feature.properties.style;
		},

		onEachFeature: onEachFeature,

		pointToLayer: function (feature, latlng) {
			return L.circleMarker(latlng, {
				radius: 8,
				fillColor: "#ff7800",
				color: "#000",
				weight: 1,
				opacity: 1,
				fillOpacity: 0.8
			});
		}
	}).addTo(map);

	L.geoJson(freeBus, {

		filter: function (feature, layer) {
			if (feature.properties) {
				// If the property "underConstruction" exists and is true, return false (don't render features under construction)
				return feature.properties.underConstruction !== undefined ? !feature.properties.underConstruction : true;
			}
			return false;
		},

		onEachFeature: onEachFeature
	}).addTo(map);

	var coorsLayer = L.geoJson(null, {

		pointToLayer: function (feature, latlng) {
			return L.marker(latlng, {icon: baseballIcon});
		},

		onEachFeature: onEachFeature
	}).addTo(map);

	coorsLayer.addData(coorsField);

</script>

<p><a href="examples/geojson/geojson-example.html">View example on a separate page &rarr;</a></p>
