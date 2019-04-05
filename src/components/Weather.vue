<template>
<div class="container">
    <div class="container-header">
        <img class="mainlogo" src="https://cdn4.iconfinder.com/data/icons/the-weather-is-nice-today/64/weather_47-512.png" alt="flower">
        <h1>The Weather App</h1>
        <input type="text" class="newCity" placeholder="Enter your city" v-model="newCity" v-on:keyup.enter="getWeather">
    </div>
    <div>
        <h2>{{forecast24message}} {{city}} {{country}}</h2>
    </div>
    <div class="dayforecast">
        <div class="hourforecast" v-for="(items,index) in dayforecastarray" :key="items.id">
            <p class="time">{{dayforecastarray[index].time}}</p>
            <img v-bind:src="dayforecastarray[index].icone">
            <p>{{dayforecastarray[index].condition}}</p>
            <div class="temperature">
                <img class="logotemp" src="../assets/iconfinder_Temperature_Cold_1084548.png" alt="thermometer">
            <p>{{dayforecastarray[index].temperature}} °C</p>
            </div>
            <div class="wind">
                <img class="logowind" src="../assets/iconfinder_wind_2016194.png" alt="wind">
                <p>{{dayforecastarray[index].wind}} km/h</p>
            </div>
            
        </div>
    </div>
    <div>
        <h2>
            {{forecastdaysmessage}} {{city}} {{country}}
        </h2>
    </div>
    <div class="weatherforecast">
        
        <div class="weatherday" v-for="(items,index) in forecastarray" :key="items.id">
            <p class="day">{{forecastarray[index].day}}</p>
            <img v-bind:src="forecastarray[index].icone">
            <p>{{forecastarray[index].temperature}} °C</p>
        </div>

    </div>
</div>
    
</template>

<script>
import axios from 'axios'
import {TweenMax} from 'gsap'

export default {

    data(){
        
    return{
        weather:null,
        forecastarray:[
        ],
        dayforecastarray:[],
        newCity:'',
        city:'',
        country:'',
        forecast24message:'',
        forecastdaysmessage:''
    }},

methods: {
    
    getWeather(newCity){
        TweenMax.staggerFrom(".hourforecast", 2, {skewY: 180,stagger:0.1})
        TweenMax.staggerFrom(".weatherday", 2, {skewY: 180,stagger:0.1})
        function getWeekDay(date){
        //Create an array containing each day, starting with Sunday.
        var weekdays = new Array(
         "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"
        )//Use the getDay() method to get the day.
        var day = date.getDay();
        //Return the element that corresponds to that index.
        return weekdays[day];}
        console.log("look for meteo")
        this.city=this.newCity
        var urlmeteo = 'https://api.openweathermap.org/data/2.5/forecast?q='+ this.newCity + '&units=metric&APPID=c0e6c657df5363d4574dd433fa03f43d'
        this.axios.get(urlmeteo).then(response => {
        this.country=response.data.city.country    
        this.newCity=''
        this.forecastarray=[]
        this.dayforecastarray=[]
        this.forecast24message="Next 24 hours forecast in",
        this.forecastdaysmessage="Next days forecast in"
            for (var i=8; i < 40; i=i+8){
                var date = new Date(response.data.list[i].dt_txt.slice(0,10));
                var weekDay = getWeekDay(date);
                 this.forecastarray.push(this.weather={
                    temperature:response.data.list[i].main.temp,
                    icone: 'http://openweathermap.org/img/w/' + response.data.list[i].weather[0].icon + '.png', 
                    day: weekDay
})
}
            for (var j=0; j < 8; j++){
                this.dayforecastarray.push(this.weather={
                    temperature:response.data.list[j].main.temp,
                    icone: 'http://openweathermap.org/img/w/' + response.data.list[j].weather[0].icon + '.png',
                    time : response.data.list[j].dt_txt.slice(11,16),
                    condition : response.data.list[j].weather[0].description,
                    wind : response.data.list[j].wind.speed


                })
            }
    })  
    },
   

}  
}

</script>

<style>

body{
    margin:0px;
    background-color: #3442A4;
    font-weight: bold; 
}

input{
    font-size: 20px;
}

h1{
    color: #FFFFFF;
    margin-right: 600px;
}

h2{
    color: #FFFFFF;
}
.container{
    max-height:1024px;
    background-color: #3442A4;

}
.container-header{
display: flex;
width: 100%;
height: 117px;
background: #242E5E;
align-items: center;
justify-content: space-between;
}
.newCity{
float:right;
width: 463px;
height: 40px;
background: #FFFFFF;
border-radius: 10px;
margin-right:50px;
}
.weatherforecast{
    position:relative;
    display: grid;
    width:100%;
    background-color:#3442A4;
    grid-template-columns: auto auto auto auto;
    grid-column-gap: 10px;
    height:200px;
    
}
.weatherday{
    margin-top:20px;
    width:190px;
    vertical-align: middle;
    background-color: #FFFFFF;
    color: black;
    height: 200px;
    margin:auto;
    border-radius: 10px;
    box-shadow: 4px 5px 11px rgba(0, 0, 0, 0.8);
    background: rgba(255, 255, 255, 0.8);
}
.dayforecast{
    display:grid;
    grid-template-columns: auto auto auto auto auto auto auto auto;
    grid-column-gap:10px;
}
.hourforecast{
    background: rgba(255, 255, 255, 0.8);
    border-radius: 10px;
    width:190px;
    margin:auto;
    height:400px;
    box-shadow: 4px 5px 11px rgba(0, 0, 0, 0.9);
    
}
img{
    width:90px;
    height:90px;
}
.temperature{
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top:40px;
}
.logotemp{
    height:50px;
    width:auto;
}

.wind{
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top:40px;
}
.logowind{
    margin-right:10px;
    height:50px;
    width:auto;
}
.mainlogo{
    margin-left:20px;
}
.time{
    font-size: 20px;
}
.day{
    font-size:20px;
}
p{
    margin-top: 10px;
}
</style>
