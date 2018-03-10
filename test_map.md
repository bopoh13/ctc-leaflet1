п»ї---
layout: page
title: Test Map
permalink: /map/
robots: noindex, nofollow
sitemap: false
sort: 1
---

<h3>Using GeoJSON with Leaflet</h3>

<p>GeoJSON is becoming a very popular data format among many GIS technologies and services - it's simple, lightweight, straightforward, and Leaflet is quite good at handling it. In this example, you'll learn how to create and interact with map vectors created from <a href="http://geojson.org/">GeoJSON</a> objects.</p>

<div id="map" class="map" style="height: 450px"></div>

<script src="../examples/sample-geojson-test.js"></script>
<script>

	var radius = 1000;
	var lat = 52.753;
	var lng = 39.298;

	var map = L.map('map', {
		minZoom: 3
	}).setView([lat, lng], 12);

	var osm = L.tileLayer(MB_URL, {
		attribution: MB_ATTR,
		id: 'mapbox.light'
	});

	osm.addTo(map);

	L.control.scale().addTo(map); // РџСЂРёРјРµСЂ РѕС‚РѕР±СЂР°Р¶РµРЅРёСЏ РјР°СЃС€С‚Р°Р±Р°


var sale = new L.LayerGroup();

L.circle([52.753, 39.298], 300, {
  stroke: false,
  fillColor: 'green',
  fillOpacity: 0.7
}).addTo(sale);

L.control.layers({
  "Open Street Map" : osm
}, {
  "just Point": sale
}).addTo(map);

	// control that shows state info on hover
	var info = L.control();

	info.onAdd = function (map) {
		this._div = L.DomUtil.create('div', 'info legend');
		this.update();
		return this._div;
	};

	info.update = function (props) {
		this._div.innerHTML = '<h4>РљРѕРѕСЂРґРёРЅР°С‚С‹ С†РµРЅС‚СЂР°</h4>' +  (props ?
			'lan: ' + props.latlng.lat + '<br />lng: ' + props.latlng.lng : 'РљРѕРѕСЂРґРёРЅР°С‚ РЅРµС‚');
	};

	info.addTo(map);


	var bounds = [[52.505, 39.23], [52.5, 39.25]];

	var rect = L.rectangle(bounds, {
		color: 'blue',
		weight: 1
	}).on('click', function (e) {
	    // There event is event object
	    // there e.type === 'click'
	    // there e.lanlng === L.LatLng on map
	    // there e.target.getLatLngs() - your rectangle coordinates
	    // but e.target !== rect
	    // info.update(e);  // bug
	    console.info(e);
	}).addTo(map);


	var baseballIcon = L.icon({
		iconUrl: '../examples/geojson/baseball-marker.png',
		iconSize: [32, 37],
		iconAnchor: [16, 37],
		popupAnchor: [0, -28]
	});

	function onEachFeature(feature, layer) {
		var popupContent = "<p>I started out as a GeoJSON <b>" +
				feature.geometry.type + "</b>, but now I'm a Leaflet vector!</p>";

		if (feature.properties && feature.properties.popupContent) {
			popupContent += feature.properties.popupContent;
		}

		layer.bindPopup(popupContent);
	}

	var LineStyle = {
		"color": "#0d0",
		"weight": 6,
		"opacity": 0.35
	};

	L.geoJson(freeBus, {

		style: LineStyle,

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

	// РњРёС€РµРЅСЊ РІ С†РµРЅС‚СЂРµ СЌРєСЂР°РЅР°
	var circle = new L.circle([lat, lng], radius, {
		color: 'red',
		fillColor: '#f03',
		fillOpacity: 0.2
	});
	map.addLayer(circle);

	map.on("move", function() {
		var center = map.getCenter();
		circle.setLatLng(center);
		console.log(circle._latlng.lat, circle._latlng.lng);
	});

        // L.marker(getRandomLatLng())
	L.marker([52.737227, 39.26419]).addTo(map).bindTooltip("my tooltip text", {direction: 'left', offset: [-250, -250]}).openTooltip();

</script>

<p><a href="../examples/geojson/geojson-example.html">View example on a separate page &rarr;</a></p>
