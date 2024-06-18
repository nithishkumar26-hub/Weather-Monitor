<svelte:head>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.8.0/dist/leaflet.css" integrity="sha512-hoalWLoI8r4UszCkZ5kL8vayOGVae1oxXe/2A4AO6J9+580uKHDO3JdHb7NzwwzK5xr/Fs0W40kiNHxM9vyTtQ==" crossorigin="" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
</svelte:head>

<script>
    // @ts-nocheck
    import L from 'leaflet'; // Import Leaflet here
	import {onMount} from "svelte"

  
    export let latitude;
    export let longitude;
       
    // 	import 'leaflet/dist/leaflet.css' // alternative to <link> if not in REPL

    // const coords = [[51.5, -0.09], [51.49, -0.08], [51.48, -0.08]]
    // const coords2 = [[51.48, -0.09], [51.485, -0.112], [51.495, -0.115]]

    onMount(() =>  {
		if (typeof window !== 'undefined') {
            const map = L.map('map', {
            maxZoom: 18, // Set the maximum zoom level
            minZoom: 2   // Set the minimum zoom level
        }).setView([latitude , longitude], 7);

        L.tileLayer('https://api.maptiler.com/maps/streets-v2/{z}/{x}/{y}.png?key=wchfx3TGhBvFM8sFDvrr', {
            attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>',
        }).addTo(map);

        const customIcon = L.icon({
            iconUrl: './maps.svg',
            iconSize: [70,70],
        })
            // coords.forEach(c => L.marker(c).addTo(map))
            // L.layerGroup(coords2.map(c => new L.Marker(c))).addTo(map)
            var markerone={};

            
            markerone = L.marker([latitude,longitude],{
            // title:"Rockfort",
            icon: customIcon,
            })
            // .bindPopup('<h2>Dindigul Rockfort</h2>')
            .addTo(map);
        console.log(map)
        // map.on('click', (event) => console.log(event))
        // L.control.zoom({
        //     zoomDelta: 0.25,
        //     zoomSnap: 2 // You can change the position of the zoom control
        // }).addTo(map);

        var theMarker = {};

        map.on('click',function(e){
        // latitude = e.latlng.lat;
        // longitude = e.latlng.lng;
		handleCoordinates(e.latlng.lat, e.latlng.lng);
        console.log("You clicked the map at LAT: "+ latitude+" and LONG: "+longitude );
            //Clear existing marker, 

            if (theMarker != undefined) {
                    map.removeLayer(theMarker);
            };

            if(markerone !=undefined){
                map.removeLayer(markerone);
            }

        //Add a marker to show where you clicked.
            theMarker = L.marker([latitude,longitude],{
                icon: customIcon,
            }).addTo(map);  
        });
        map.on('contextmenu', (event) => console.log('Set Marker?'))
	}
    });

	function handleCoordinates(lat, lon) {
        latitude = lat;
        longitude = lon;
    }
   
</script>

<div id="map"></div>

<style>
    #map {
        height: 600px;
        width: 400px;
    }
</style>
