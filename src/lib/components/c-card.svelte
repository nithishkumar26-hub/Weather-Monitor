<script>
// @ts-nocheck

    import Input from "../components/c-input.svelte"
    import axios from "axios";
    import BASE_URL from "../../baseurl";
    // import { Map } from "maplibre-gl";
    
    let Appid = '902317cb85634f3b2263face38b21428'
    let Appid1="1a2d9af69ffe427b9903dd46a54a4359"
    let inputValue='';
    let weatherdata=[];
    let wtdata="";
    let weathercelsius=0;
    let maxtemp=0;
    let mintemp=0;
    function clicktocheck(){
        getData()
    }
 
    function getData(){
        let config = {
        method: 'get',
        maxBodyLength: Infinity,
        url: `${BASE_URL}/data/2.5/weather?q=${inputValue}&appid=${Appid}`,
        headers: { }
        };

        axios.request(config)
        .then((response) => {
            weatherdata=response.data;
            wtdata=weatherdata.weather[0].main
            console.log("weatherdata",weatherdata)
            console.log("wtdata",wtdata)
            kelvintocelsius()
        console.log(JSON.stringify(response.data));
        })
        .catch((error) => {
        console.log(error);
        });

    }

    function kelvintocelsius(){
        weathercelsius=(weatherdata.main.temp-273.15).toFixed(2);
        maxtemp=(weatherdata.main.temp_max-273.15).toFixed(2);
        mintemp=(weatherdata.main.temp_min-273.15).toFixed(2);
        console.log("weathercelsius",weathercelsius)
    }

    

    // const map = new Map({
    // container: "my-map",
    // style: `https://maps.geoapify.com/v1/styles/klokantech-basic/style.json?apiKey=${Appid1}`,
    // });

    // map.on("click", "places", function (e) {
    // var coordinates = [e.lngLat.lng, e.lngLat.lat];

    // // on small zoom levels it could happen that a location is present multiple times on the map
    // while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
    //     coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
    // }

    // fetch(`https://api.geoapify.com/v1/geocode/reverse?lat=${coordinates[1]}&lon=${coordinates[0]}&apiKey=${Appid1}`, requestOptions)
    // .then(response => response.json())
    // .then(result => {
    //     if (result.features.length) {
    //     const address = result.features[0].properties.formatted;
    //     new mapboxgl.Popup().setLngLat(coordinates).setHTML(address).addTo(map);
    //     } else {
    //     new mapboxgl.Popup().setLngLat(coordinates).setHTML("No address found").addTo(map);
    //     }
    // });
    // });

</script>

<div class=" w-[300px] h-[400px] mx-auto p-5 text-center rounded-[30px] custom-gradient">
    <div class="font-poppins font-medium text-[20px]">Weather Monitor</div>
    <div class="flex flex-col mt-[50px]">
        <Input type="text" placeholder="Enter City Name" bind:value={inputValue}  />
        <button class="font-poppins w-[160px] h-[30px] border-none bg-[#A3A9DF] text-white text-center 
        rounded mx-auto  mt-[10px]" on:click={()=>clicktocheck()}> Click here</button>
        {#if weatherdata.name !== undefined }
            <div class="mt-[10px]">{weatherdata.name}</div>
        {:else}
            <div class="mt-[10px]">------</div>
        {/if}
        {#if wtdata !== "" }
            <div class="mt-[10px]">{wtdata}</div>
        {:else}
            <div class="mt-[10px]">------</div>
        {/if}
        <div class="font-poppins flex flex-col items-start mt-[15px] ">
            <div class=" flex flex-row">Max-Temp:  
                <div class="ml-[60px]">{maxtemp}°C</div>
            </div>
            <div class="mt-[10px] flex flex-row">Min-Temp: 
                <div class="ml-[64px]">{mintemp}°C</div>
            </div>
        </div>
    </div>
</div>
<div>
    <!-- <Map
    id="map"
    style=""/> -->

  </div>
<style>
    .custom-gradient{
        background: linear-gradient(135deg, #00febe, #5b548a);
    }
</style>