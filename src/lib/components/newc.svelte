<script>
// @ts-nocheck

  import { onMount } from "svelte";
import axios from "axios";
   
let responsedata=[];
   onMount(()=>{
    let config = {
    method: 'get',
    maxBodyLength: Infinity,
    url: 'http://dataservice.accuweather.com/forecasts/v1/daily/5day/190799?apikey=%09G8kCkgW0gC4WOiixTb2JU0qD83Ax7T8F&metric=true',
    headers: { }
    };

    axios.request(config)
    .then((response) => {
        responsedata=response.data.DailyForecasts[0].Day.Icon;
        console.log("responsedata",responsedata)
    console.log(JSON.stringify(response.data));
    })
    .catch((error) => {
    console.log(error);
    });
   })

 

</script>

<div class="w-10 h-10">
    <div>{responsedata}</div>
</div>