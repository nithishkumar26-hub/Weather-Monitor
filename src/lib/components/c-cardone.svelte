
<script>
    // @ts-nocheck
    
        import Input from "../components/c-input.svelte"
        import axios from "axios";
        import BASE_URL from "../../baseurl";
        import Table from "../components/c-table.svelte"
        import Map from "../components/c-map.svelte"
        import Chart from "../components/c-chart.svelte"
        import { theme } from "../../store";
        import { onMount } from "svelte";
        let Appid = '902317cb85634f3b2263face38b21428'
        let Appid1='8aebab06dc234080bdf145006242802'
        let Appid2='a5becccb44731a7f30551a67d6fea497'

        //api key for accuweather
        let Appidaccu="%09G8kCkgW0gC4WOiixTb2JU0qD83Ax7T8F"
        let inputValue='';
        let weatherdata=[];
        // get day for accuweather
        let accuweatherdata=[];
        let citykey;
        //base url for Accuweather
        let BASE_URL1 = "http://dataservice.accuweather.com"

        let datasofdays=[];
        let filteredForecast=[];
       
        let locationdata=[];
        let wtdata="";
        let weathercelsius=0;
        let maxtemp=0;
        let mintemp=0;
        let realfeel=0;
        let kmperhour=0;
        let hour=0;
        let date="";
        let weekday="";
        let sunrise="";
        let sunset="";
        let isOpen=false;
        let openMap=false;
        let latitude=10.351734510058574 
      
        let longitude=77.9831886291504
        let clickOk=false;
       
        
        function clicktocheck(){
            getData()
            // get5daydata()
            getdatasof5day()
            getAccuweather()
        }

        //get weather data from the accuweather api
        function getAccuweather(){
            let config = {
            method: 'get',
            maxBodyLength: Infinity,
            url: 'https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/dindigul/next7days?unitGroup=metric&include=days%2Ccurrent&key=GW8Y6JJ3D3S42JYUX26TLX42E&contentType=json',
            headers: { }
            };

            axios.request(config)
            .then((response) => {
                accuweatherdata=response.data;
                console.log("accuweather",accuweatherdata)
            console.log(JSON.stringify(response.data));
            })
            .catch((error) => {
            console.log(error);
            });

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
                convertUnixToIST()
                unitconversion()
            console.log(JSON.stringify(response.data));
            })
            .catch((error) => {
                 weatherdata=error.response.data;
                 console.log("weatherdata",weatherdata)
                console.log(error);
            });
    
        }

        function preciseLocation(){
            if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition(function(position) {
                latitude = position.coords.latitude;
                longitude = position.coords.longitude;
                console.log(`Latitude: ${latitude}, Longitude: ${longitude}`);
                getLocation()
            });
            } 
            else {
            console.log("Geolocation is not supported by this browser.");
            }
        }

        function getmapLocation(){
            openMap=!openMap
           
        }
        $: if(latitude && longitude){
                getLocation()
            }

        function getLocation(){
            
            let config = {
            method: 'get',
            maxBodyLength: Infinity,
            url: `${BASE_URL}/geo/1.0/reverse?lat=${latitude}&lon=${longitude}&limit=1&appid=${Appid2}`,
            headers: { }
            };

            axios.request(config)
            .then((response) => {
                locationdata=response.data;
                console.log("locationdata",locationdata)
                inputValue=locationdata[0].name;
                getData()
                getdatasof5day()
                getAccuweather()
            console.log(JSON.stringify(response.data));
            })
            .catch((error) => {
            console.log(error);
            });

        }

        let items=
        [
            // {name:'A1882142', id:'PCS',no:" 36"},
            // {name:'A1882148',id:'PCS',no:"27"},
            // {name:'A1885526',id:'PCS',no:"100"},
            // {name:'A1882782',id:'PCS',no:"250"}
        ]

    

        let columns=
        [
            {label:'', key:'name'},
            {label:"Max-Temp",key:"max"},
            {label:"Min-Temp",key:"min"},
        ]

        let chartData=[]

        let labels=[]

        let avatarArray=[]
        // function get5daydata(){

        //     let config = {
        //     method: 'get',
        //     maxBodyLength: Infinity,
        //     url: `${BASE_URL}/data/2.5/forecast?q=${inputValue}&appid=${Appid1}`,
        //     headers: { }
        //     };

        //     axios.request(config)
        //     .then((response) => {
        //         fivedaydata=response.data;
               
        //         // const uniqueforecastdays=[];
        //         // const fivedayforecast=fivedaydata.list.filter(forecast =>{
        //         //     const forecastdate=new Date(forecast.dt_txt).getDate();
                   
        //         //     if(!uniqueforecastdays.includes(forecastdate)){
        //         //         return uniqueforecastdays.push(forecastdate);
        //         //     }
        //         // })
        //         // console.log("fivedayforecast",fivedayforecast)
        //         console.log("fivedaydata",fivedaydata)
        //         const targetTime = "12:00:00";
        //         filteredForecast = filterForecastByTime(fivedaydata.list, targetTime);
        //         function filterForecastByTime(forecastData, targetTime) {
        //             const forecastWithIST= forecastData.filter(entry => entry.dt_txt.endsWith(targetTime));
        //             const filteredForecast =  convertAllUnixTimestampsToIST(forecastWithIST);
        //             return filteredForecast
        //         }
        //         console.log(filteredForecast);
        //         // items=filteredForecast;
        //         // const forecastWithIST = convertAllUnixTimestampsToIST(filteredForecast);
        //         // console.log("forecastWithIST",forecastWithIST)
        //         // dateValuesArray = forecastWithIST.map(dayObject => dayObject.dt);
        //         // console.log("dateValuesArray",dateValuesArray);
        //     // console.log(JSON.stringify(response.data));
        //     })
        //     .catch((error) => {
        //     console.log(error);
        //     });

        // }

        function getdatasof5day(){
            
            let config = {
            method: 'get',
            maxBodyLength: Infinity,
            url: `http://api.weatherapi.com/v1/forecast.json?key=${Appid1}&q=${inputValue}&days=5&aqi=yes&alerts=no`,
            headers: { }
            };

            axios.request(config)
            .then((response) => {
            datasofdays=response.data;
            console.log("datasofdays",datasofdays)
            const getdata = convertAllUnixTimestampsToIST1(datasofdays.forecast.forecastday)
            console.log("getdata",getdata)
            const hrdata=getdata[0].hour
            console.log("hrdata",hrdata)
            // chartData.push(...hrdata.temp_c)
            
            chartData=hrdata.filter((item,index) => index % 3 ===0).map(item => +item.temp_c.toFixed(0));
            console.log("chartData", chartData)
            
            labels=hrdata.filter((item,index) => index % 3 === 0).map(item=>{
                let [date, time]=item.time.split(' ');
                return time;
            })
            console.log("labels",labels)

            avatarArray= hrdata.filter((item,index)=> index %3 ===0).map(item => item.condition.icon)
            console.log("icon",avatarArray)
          
            items=getdata;
        
            // console.log(JSON.stringify(response.data));
            })
            .catch((error) => {
            console.log(error);
            });

        }

        

        
    
        function kelvintocelsius(){
            weathercelsius=(weatherdata.main.temp-273.15).toFixed(0);
            realfeel=(weatherdata.main.feels_like-273.15).toFixed(0);
            maxtemp=(weatherdata.main.temp_max-273.15).toFixed(2);
            mintemp=(weatherdata.main.temp_min-273.15).toFixed(2);
            console.log("weathercelsius",weathercelsius)
        }

       
        function convertUnixToIST() {
            // Convert Unix timestamp to milliseconds
            const unixMilliseconds = (weatherdata.dt) * 1000;

            // Create a Date object using the Unix timestamp
            const dateObject = new Date(unixMilliseconds);
            console.log("dateObject",dateObject)
            // Define options for formatting
            const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
            const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
            const day = dateObject.getDate();
            const month = months[dateObject.getMonth()];
            const year = dateObject.getFullYear();
            const dayOfWeek = days[dateObject.getDay()];
            console.log('IST Time:',`${day}-${month}-${year} ${dayOfWeek}`);
            date = `${day}-${month}-${year}`
            weekday=`${dayOfWeek}`
            hourvalue()
            
            //sunrise value
            const unixMilliseconds1=(weatherdata.sys.sunrise) * 1000;
            const timeObject = new Date(unixMilliseconds1);

            const hours = timeObject.getHours();
            const minutes = timeObject.getMinutes();
            console.log("Sunrise",`${hours}:${minutes}`)
            sunrise=`${hours}:${minutes} am`
            
            //sunset value
            const unixMilliseconds2=(weatherdata.sys.sunset) * 1000;
            const timeObjectset = new Date(unixMilliseconds2);

            let hoursset = timeObjectset.getHours();
            const minutesset = timeObjectset.getMinutes();

            hoursset = hoursset % 12 || 12;
            console.log("Sunset",`${hoursset}:${minutesset}`)
            sunset=`${hoursset}:${minutesset} pm`

            
        }

        // function convertAllUnixTimestampsToIST(filteredForecast) {
        //     return filteredForecast.map(entry => {
        //         return {
        //         ...entry,
        //         dt: convertUnixTimestampToIST(entry.dt),
        //         };
        //     });
        // }

        function convertAllUnixTimestampsToIST1(filteredForecast) {
            return filteredForecast.map(entry => {
                return {
                ...entry,
                date_epoch: convertUnixTimestampToIST(entry.date_epoch),
                };
            });
        }

        function convertUnixTimestampToIST(unixTimestamp) {
            const unixMilliseconds = unixTimestamp * 1000;
            const dateObject = new Date(unixMilliseconds);
            const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
            const dayOfWeek = days[dateObject.getDay()];
            console.log("daysofweek",dayOfWeek)
            return dayOfWeek
        
        }


        function hourvalue(){
            const unixMilliseconds = (weatherdata.dt) * 1000;
            // Create a Date object using the Unix timestamp
            const dateObject = new Date(unixMilliseconds);
            // Get the hour from the Date object
            hour = dateObject.getHours();
            console.log("hourvalue",hour)
        }

        function unitconversion(){
            kmperhour = (weatherdata.wind.speed*3.6).toFixed(0);
            console.log( "kmperhour",kmperhour)
        }

        function openBar(){
            isOpen=!isOpen
        }

        function changeTheme(){
            $theme=!$theme
        }

        // let bgimage = $theme ? 'darkbgimage.jpg' : 'mypics.png';
        

        
       

</script>
<main class={`${$theme && "dark"}`}  style="background-image: url({$theme ? 'darkbgimage.jpg' : 'mypics.png'})">
    <div class="flex flex-row pt-[20px] overflow-hidden "  >
        
        <div class=" w-[30%] h-[50%] ml-[20px] p-5 text-center rounded-[30px] custom-gradient bg-[rgba(0,0,0,0.2)] dark:bg-[#010101]">
            <!-- <div class="font-poppins font-medium text-4xl text-white tracking-tight mt-5">Weather Monitor</div> -->
            <div class="flex flex-col">
                <!-- <button class="font-poppins w-[150px] h-8 text-black text-sm bg-blue-400 dark:bg-purple-400 dark:text-white"
                on:click={()=> changeTheme()}>clickme!</button> -->
                <div class="flex justify-end">
                    <label for="dark-mode" class=" h-8 w-16 relative block bg-[#ebebeb] rounded-[200px] cursor-pointer">
                        <input type="checkbox" id="dark-mode" class="sr-only peer" on:click={()=> changeTheme()}/>
                            <div class="w-2/5 h-4/5 bg-rose-300 absolute rounded-full left-1 top-1 
                            transition-all duration-300 peer-checked:bg-red-600 peer-checked:left-8" ></div>
                    </label>
                </div>
                <div  class="flex flex-row mb-[10px] mt-[30px]">
                    <Input type="text" placeholder="Enter City Name" style="w-[90%] flex items-start" bind:value={inputValue} />
                    <button class="w-[40px] h-[40px] bg-white rounded-full ml-[-25px] " on:click={()=>clicktocheck()}>
                        <img src="/search.svg" alt="search" class="w-[25px] h-[25px] pl-[8px] "/>
                    </button>
                    
                </div>
                <div class="flex flex-row">
                    <div class="border border-solid border-[#fff] m-[20px] w-[200px] h-0 "/>
                    <span class="font-poppins text-sm text-white flex items-center ">OR</span>
                    <div class="border border-solid border-[#fff] m-[20px] w-[200px] h-0 "/>
                </div> 
                <button class="w-8 h-8 rounded-full bg-white flex items-center ml-[415px] mt-[-40px] z-[999]" on:click={()=>openBar()}>
                    <img src="/menu-right.svg" alt="right" class="w-[35px] h-[35px]"/>
                </button> 
                <div class="flex flex-row">
                    <button class="w-full mx-auto h-[40px] bg-[#2091F0] border-none rounded-xl flex justify-center items-center"
                        on:click={()=>preciseLocation()}>
                        <img src="/target.svg" alt="location target" class="w-[20px] h-[20px]"/>
                        <span class="font-poppins text-sm text-white pl-[8px]">Use precise location</span>
                    </button>
                    <button class="w-full mx-auto h-[40px] bg-[#2091F0] border-none rounded-xl flex justify-center items-center ml-[10px]"
                        on:click={()=>getmapLocation()}>
                        <img src="/location.svg" alt="location target" class="w-[20px] h-[20px]"/>
                        <span class="font-poppins text-sm text-white pl-[8px]">Use Map</span>
                    </button>
                </div>
                {#if weatherdata.cod === "404"}
                    <div class="flex items-start text-sm text-white font-poppins">Invalid city name</div>
                <!-- svelte-ignore empty-block -->
                {:else}
            
                    <!-- <div class="text-xl text-white font-poppins">{istTime}</div> -->
                    <!-- svelte-ignore empty-block -->
                    {#if weatherdata.name === undefined}

                    {:else}
                        <div class="h-[260px] mx-auto w-full bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl mt-[15px]">
                            <div class="flex justify-center">
                                <!-- {#if wtdata == "Clear"  && 6< hour <19}
                                    <img src="/sun.png" alt="drizzle" class="w-[100px] h-[100px]"/>
                                {:else if wtdata == "Clear"  && 19<hour<6}
                                    <img src="/moon.png" alt="drizzle" class="w-[100px] h-[100px]"/>
                                {:else if wtdata == "Clouds" && 6< hour <19}
                                    <img src="/cloudy.png" alt="drizzle" class="w-[100px] h-[100px]"/>
                                {:else if wtdata == "Clouds" && 19<hour<6 }
                                    <img src="/cloudy-night.png" alt="drizzle" class="w-[100px] h-[100px]"/>
                                {:else if wtdata == "Mist" && 6< hour <19}
                                    <img src="/mist.png" alt="drizzle" class="w-[100px] h-[100px]"/>
                                {:else if wtdata == "Mist" && 19<hour<6 }
                                    <img src="/mist-moon.png" alt="drizzle" class="w-[100px] h-[100px]"/>
                                {/if} -->
                                <!-- svelte-ignore a11y-img-redundant-alt -->
                                <img src={`http://openweathermap.org/img/wn/${weatherdata.weather[0].icon}@2x.png`} alt="weather image" 
                                class="w-[160px] h-[160px]"/>
                            </div> 
                            <div class="flex flex-row justify-center mt-[-20px]">
                                <span class="font-poppins text-4xl text-white ">{weathercelsius}</span> 
                                <span class="font-poppins text-2xl flex items-start text-white ">°C</span>
                                <span class="font-poppins text-xl text-white ml-[10px]  flex items-end">{wtdata}</span>
                            </div>
                            <div class="font-poppins text-4xl text-white flex justify-center mt-[20px]">{weatherdata.name}</div>
                        </div>

                      
                        <hr class="border border-solid border-[#fff] m-[20px] "/>
                        <div class="h-[100px] mx-auto w-full bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl flex flex-col justify-center mb-2">
                            <div class="text-xl text-white font-poppins">{date}</div>
                            <div class="text-xl text-white font-poppins">{weekday}</div>
                        </div>
                    {/if}
                {/if}
            </div>
        
        </div>
        {#if openMap}
            <Map bind:latitude bind:longitude />
        {/if}
        <!-- <p>Latitude: {latitude}</p>
        <p>Longitude: {longitude}</p> -->
        {#if isOpen && weatherdata.name !== undefined}
            <div class=" w-[60%] h-[50%] flex  p-5 text-center rounded-[30px] custom-gradient bg-[rgba(0,0,0,0.2)] dark:bg-[#010101] transform 
            transition-transform duration-300 ease-in">
                <div class="flex flex-col w-full ">
                    <div class="flex flex-row w-full flex-shrink ">            
                        <div class="flex flex-col w-[100%] mx-auto m-2 ml-[25px] ">
                            
                            <div class=" h-[240px] w-full bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl mx-auto p-3  ">
                                <Chart  {labels} {chartData} {avatarArray}/>
                            </div>
                            <div class="h-[160px]  w-full bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl justify-center items-start p-3 mt-[40px]">
                                <div class="flex flex-row justify-center">
                                    <div class="flex flex-col mr-[20px]">
                                        <img src="/sunrise.svg" alt="humidity" class="w-[100px] h-[80px] mt-[-10px]"/> 
                                        <span class="font-poppins flex justify-center text-xl text-white "> {sunrise}</span>
                                        <span class="font-poppins flex justify-center text-xl text-white "> sunrise</span>
                                    </div>
                                    <div class="flex flex-col">
                                        <img src="/sunset.svg" alt="humidity" class="w-[100px] h-[80px] mt-[-10px]"/> 
                                        <span class="font-poppins flex justify-center text-xl text-white "> {sunset}</span>
                                        <span class="font-poppins flex justify-center text-xl text-white"> sunset</span>
                                    </div>
                                </div>
                            </div>
                            <!-- <div class=" h-[160px] w-full bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl mx-auto p-3 mt-[30px] flex justify-center">
                                <Table {items} {columns} />
                            </div> -->
                        
                        </div>

                        <div class="flex flex-col w-[100%] mx-auto m-2 ml-[25px]">
                            
                            <div class="flex flex-row  m-2 ">
                                <div class="flex flex-col w-full ">
                                    <div class="h-[130px] bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl p-4 ">
                                        <span class="font-poppins text-xl flex justify-start text-white ">Humidity</span>
                                        <div class="flex flex-row mt-[20px]">
                                            <img src="/humidity.svg" alt="humidity" class="w-[40px] h-[40px]"/> 
                                            <span class="font-poppins text-xl  text-white ml-[15px] flex items-center"> 
                                                {weatherdata?.main?.humidity}%</span>
                                        </div>
                                    </div>
                                </div>
                                <div class="flex flex-col ml-[25px] w-full">
                                    <div class="h-[130px]  bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl  p-4">
                                        <span class="font-poppins text-xl text-white flex justify-start">wind speed</span>
                                        <div class="flex flex-row mt-[20px]">
                                            <img src="/windy.svg" alt="humidity" class="w-[40px] h-[40px]"/> 
                                            <span class="font-poppins text-xl text-white ml-[15px] flex items-center">{kmperhour} km/h</span>
                                        </div>    
                                    </div>
                                </div>
                            </div>

                            <div class="flex flex-row m-2">
                                <div class="flex flex-col w-full ">
                                    <div class="h-[130px] bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl justify-center items-start p-4 ">
                                        <span class="font-poppins text-xl text-white flex justify-start ">Real Feel</span>
                                        <div class="flex flex-row mt-[20px]">
                                            <img src="/weather-meter.svg" alt="humidity" class="w-[40px] h-[40px]"/> 
                                            <span class="font-poppins text-3xl text-white ml-[15px]"> {realfeel}</span>
                                            <span  class="font-poppins text-xl text-white ">°C</span>
                                        </div>
                                    </div>
                                </div>
                                <div class="flex flex-col ml-[25px] w-full">
                                    <div class="h-[130px] bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl justify-center items-start p-4">
                                        <span class="font-poppins text-xl text-white flex justify-start">Pressure</span>
                                        <div class="flex flex-row mt-[20px]">
                                            <img src="/barometer.svg" alt="humidity" class="w-[40px] h-[40px]"/> 
                                            <span class="font-poppins text-xl text-white ml-[15px]">{weatherdata?.main?.humidity} hPa</span>
                                        </div>    
                                    </div>
                                </div>
                            </div>

                            <div class="flex flex-row m-2">
                                <div class="flex flex-col w-full ">
                                    <div class="h-[130px] bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl justify-center items-start p-5 ">
                                        <span class="font-poppins text-xl text-white flex justify-start ">AQI</span>
                                        <div class="flex flex-row mt-[20px] items-center">
                                            <img src="/air-quality-index.svg" alt="humidity" class="w-[40px] h-[40px]"/> 
                                            <span class="font-poppins text-xl text-white flex justify-center ml-[15px]"> 
                                                {#if datasofdays?.current?.air_quality?.pm2_5 < 25}
                                                    <span>Good</span>
                                                {:else if 25 < datasofdays?.current?.air_quality?.pm2_5 < 50}
                                                    <span>Fair</span>
                                                {:else if 50 < datasofdays?.current?.air_quality?.pm2_5 < 100}
                                                    <span>Poor</span>
                                                {:else if 100 < datasofdays?.current?.air_quality?.pm2_5 < 500}
                                                    <span>Very Poor</span>
                                                {/if}
                                            </span>
                                            
                                        </div>
                                    </div>
                                </div>
                                <div class="flex flex-col ml-[25px] w-full">
                                    <div class="h-[130px] bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl justify-center items-start p-5">
                                        <span class="font-poppins text-xl text-white flex justify-start">UV Index</span>
                                        <div class="flex flex-row mt-[20px] items-center">
                                            <img src="/uv-index.svg" alt="humidity" class="w-[40px] h-[40px]"/> 
                                            <span class="font-poppins text-xl text-white ml-[15px]">
                                                {#if 1<= datasofdays?.current?.uv <= 2}
                                                    <span>Low</span>
                                                {:else if 3 <= datasofdays?.current?.uv <= 5}
                                                    <span>Moderate</span>
                                                {:else if 6 <= datasofdays?.current?.uv <= 7}
                                                    <span>High</span>
                                                {:else if 8 <= datasofdays?.current?.uv <= 10}
                                                    <span>Very High</span>
                                                {:else if  datasofdays?.current?.uv >= 11}
                                                    <span>Extreme</span>
                                                {/if}
                                            </span>
                                        </div>    
                                    </div>
                                </div> 
                            </div>

                            <!-- <div class="flex flex-row m-2">
                                <div class="flex flex-col w-full ">
                                    <div class="h-[130px] bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl justify-center items-start p-4 ">
                                        <span class="font-poppins text-xl text-white flex justify-start ">wind gust</span>
                                        <div class="flex flex-row mt-[20px]">
                                            <img src="/gust.svg" alt="humidity" class="w-[40px] h-[40px]"/> 
                                            <span class="font-poppins text-xl text-white ml-[15px]"> {(datasofdays?.current?.gust_kph).toFixed(0)}</span>
                                            <span  class="font-poppins text-xl text-white ml-1">Km/hr</span>
                                        </div>
                                    </div>
                                </div>
                                <div class="flex flex-col ml-[25px] w-full">
                                    <div class="h-[130px]bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl justify-center items-start p-4">
                                        <span class="font-poppins text-xl text-white flex justify-start">visibility</span>
                                        <div class="flex-row mt-[20px] flex items-center">
                                            <img src="/visibility.svg" alt="humidity" class="w-[40px] h-[40px]"/> 
                                            <span class="font-poppins text-xl text-white ml-[15px]">{datasofdays?.current?.vis_km} Km</span>
                                        </div>    
                                    </div>
                                </div>
                            </div> -->

                        </div>
                    </div>
                    <div class="flex flex-row w-full m-2 ml-[25px] ">
                        {#each items as item}
                            <div class="w-[50%] flex ">
                                <div class="h-[18   0px]  bg-[rgba(0,0,0,0.4)] dark:bg-[#252525] rounded-xl p-4 flex flex-col">
                                    <!-- svelte-ignore a11y-img-redundant-alt -->
                                    <div class="mt-[-25px]">
                                        <img src={item.day.condition.icon} alt="weather image" 
                                        class="w-[120px] h-[120px]"/>
                                    </div>
                                    <div class="flex flex-row justify-center">
                                        <div class="font-poppins text-lg text-white">{(item.day.maxtemp_c).toFixed(0)} °C</div>
                                        <div class="font-poppins text-lg text-white"> / {(item.day.mintemp_c).toFixed(0)} °C</div>
                                    </div>
                                    <div class="font-poppins text-xl text-white mt-1">{item.date_epoch}</div>
                                </div>
                               
                               
                            </div>
                        {/each}
                    </div>
                </div>
            </div>
        {/if}
    </div>
</main>
    
<style>
    main {
        /* background: url({bgimage}) center/cover no-repeat;
        background-repeat: no-repeat; */
        height: 100vh; /* Adjust the height as needed */
        width: 100vw;
        background-position: center;
        background-size: cover;
        max-height: 100%;
    }
</style>