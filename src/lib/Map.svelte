<script lang="ts">
    import mapboxgl from "mapbox-gl";
    import "mapbox-gl/dist/mapbox-gl.css";
    import { onMount } from "svelte";

    let speed = $state("--");
    let maxSpeed = $state("--");

    onMount(() => {
        mapboxgl.accessToken =
            "pk.eyJ1Ijoic29sdnIiLCJhIjoiY203YWE5ZWhpMDIwMzJtcXgyeGRmYnBmeCJ9.6ynm11rEqIXm63MFDpWtQA";

        const map = new mapboxgl.Map({
            container: "map",
            style: "mapbox://styles/solvr/cm7acsfnu003901qyftry0tsf",
            center: [4.9041, 52.3676], // Amsterdam
            zoom: 15,
            pitch: 60,
            bearing: 0,
            antialias: true,
        });

        map.on("load", () => {
            map.addSource("mapbox-dem", {
                type: "raster-dem",
                url: "mapbox://mapbox.mapbox-terrain-dem-v1",
                tileSize: 512,
                maxzoom: 14,
            });
            map.setTerrain({ source: "mapbox-dem", exaggeration: 1.5 });
        });

        const marker = new mapboxgl.Marker({ color: "red" })
            .setLngLat([4.9041, 52.3676])
            .addTo(map);

        function updatePosition(position: GeolocationPosition) {
            let lat = position.coords.latitude;
            let lng = position.coords.longitude;
            speed = position.coords.speed
                ? (position.coords.speed * 3.6).toFixed(1)
                : "N/A";
            let heading = position.coords.heading || 0;

            marker.setLngLat([lng, lat]);
            map.easeTo({
                center: [lng, lat],
                bearing: heading,
                pitch: 60,
                duration: 1000,
            });

            getSpeedLimit(lat, lng);
        }

        function getSpeedLimit(lat: number, lng: number) {
            const overpassQuery = `[out:json];way(around:100,${lat},${lng})[maxspeed];out;`;
            const url = `https://overpass-api.de/api/interpreter?data=${encodeURIComponent(overpassQuery)}`;

            fetch(url)
                .then((res) => res.json())
                .then((data) => {
                    if (data.elements.length > 0) {
                        maxSpeed = data.elements[0].tags.maxspeed || "N/A";
                    } else {
                        maxSpeed = "N/A";
                    }
                })
                .catch(() => (maxSpeed = "N/A"));
        }

        if (navigator.geolocation) {
            navigator.geolocation.watchPosition(updatePosition, console.error, {
                enableHighAccuracy: true,
            });
        } else {
            alert("GPS wordt niet ondersteund door je browser.");
        }
    });
</script>

<div id="map" class="w-full h-screen"></div>
<div
    class="absolute top-2.5 left-2.5 bg-black bg-opacity-70 text-white p-2.5 rounded text-base"
>
    Snelheid: {speed} km/u | Max: {maxSpeed} km/u
</div>
