
<div class=" w-[100%] max-w-full relative h-[100vh]  ">
        <canvas id="myChart"  bind:this={ctx}></canvas>
    </div>

<svelte:head>
    
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.0.0-rc.1/dist/chartjs-plugin-datalabels.min.js" integrity="sha256-Oq8QGQ+hs3Sw1AeP0WhZB7nkjx6F1LxsX6dCAsyAiA4=" crossorigin="anonymous"></script>
    
</svelte:head>

<script >
// @ts-nocheck

let ctx 
import {onMount} from "svelte";
import Chart from 'chart.js/auto';
import ChartDataLabels from 'chartjs-plugin-datalabels';
// let chartData=[12, 19, 3, 5, 2, 3, 12, 19, 3, 5, 2, 3, 12, 19, 3, 5, 2, 3];
// let labels=['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange','Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange','Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'];
export let chartData;
export let labels;
export let avatarArray;
// Chart.defaults.font.size = 12;

onMount(()=>{

    //setup
    const data = {
            labels: labels,
            datasets: [{
                label: 'celsius',
                data: chartData,
                backgroundColor: 'rgba(255, 206, 86, 0.2)',
                borderColor: 'rgba(255, 206, 86, 1)',
                borderWidth: 1,
                tension: 0.2,
                borderWidth: 2,
                radius: 5,
                padding: 3,
                pointHoverRadius: 8,
                pointBackgroundColor:'yellow',
                color:'white',
                fill: true,
                backgroundColor: 'rgb(255 206 86 / 40%)',
                font:{
                    family:"sans-serif"
                },
                datalabels:{
                    color:'yellow',
                    anchor:'end',
                    align:'top',
                    offset: 5,
                    font:{
                        size: 12
                    },
                    formatter: function(value){
                        return value + '°C';
                    }
                }    
            }]
        };

        //plugin block
        const legendMargin ={
            id:"legendMargin",
            beforeInit(chart,legend,options){
                const fitValue =chart.legend.fit;

                chart.legend.fit = function fit(){
                    fitValue.bind(chart.legend)();
                    return this.height += 45;
                }
            }
        };

        const img1=new Image();
        img1.src='https://cdn.weatherapi.com/weather/64x64/day/116.png'

        console.log(img1)   
           
       

        const barAvatar={
            id: "barAvatar",
            afterDatasetDraw(chart,args, options){
                const { ctx, chartArea: {top, bottom, left, right, borderWidth, height},
                scales:{ x, y}} = chart;
                ctx.save();

                avatarArray.forEach((imgUrl, i)=>{
                    const img= new Image();
                    img.onload= () =>{
                        ctx.drawImage(img, x.getPixelForValue(i) - (30/2),  y.getPixelForValue(chartData[i]) - 60, 30,30)
                    };
                    img.src=imgUrl
                })
               
                ctx.restore();
                       
                
            }
        }

       

        //config
        const  config={
        type: 'line',
        data,
        options: {
            scales: {
                x:{
                    ticks:{
                        color:'white'
                    },
                    grid:{
                        display: false
                    },
                    title:{
                        display: true,
                        text: 'Time (24hrs format)',
                        color: 'white'
                    }
                },
                y: {
                    beginAtZero: true,
                    ticks:{
                        color:'white',
                        padding: 10,
                        callback: function(value, index, values){
                            return `${value} °C`
                        }
                    },
                    padding: 10,
                    grid:{
                        display:false
                    },
                    title:{
                        display: true,
                        text:'Celsius value',
                        color:'white'
                    },
                    font:{
                        family:'sans-serif'
                    }
                },
                // maintainAspectRatio: false,
            },
            plugins:{
                legend:{
                    labels:{
                        color:'white',
                        font:{
                            size: 16
                        },
                        padding: 10
                    }
                },
            }
            
        },
         plugins: [ChartDataLabels,legendMargin,barAvatar]
    };
    
    //render init block
    const myChart = new Chart(ctx, config);
    
})


     


</script>
