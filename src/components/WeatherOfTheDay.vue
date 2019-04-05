<template>
<div class="container">
    <div class="container-header">
        <h1>
            The Weather App
        </h1>
        <input type="text" class="newCity" placeholder="Enter your city" v-model="newCity" v-on:keyup.enter="getWeather">
    </div>
    <div class="weatherforecast" v-if="weather">
        <h1>Current Weather in {{weather.name}}</h1>
            <img v-bind:src="this.weather.image">
            <p>{{weather.weather[0].main}} : {{weather.weather[0].description}}</p>
            <p>Temperature : {{weather.main.temp}}°C</p>
        </div>
    </div>
    
</template>

<script>
import axios from 'axios'

export default {

    data(){
    return{
        weather:null,
        newCity:''
    }},

methods: {
    
    getWeather(newCity){
        console.log("look for meteo")
        var url = 'https://api.openweathermap.org/data/2.5/weather?q='+ this.newCity + '&units=metric&APPID=c0e6c657df5363d4574dd433fa03f43d'
        return axios.get(url).then(response => {
            this.weather=response.data
            this.newCity=''
            this.weather.image='http://openweathermap.org/img/w/' + this.weather.weather[0].icon + '.png'
            // this.weather.temperature=response.data.list[0].main.temp
            // this.weather.date=response.data.list[0].dt_txt
            // console.log(this.weather.temperature)
            // console.log(this.weather
    })
    
    }
},


}

</script>

<style>

h1{
    color: #FFFFFF;
    float:left;
    margin-left:100px;
}
.container-header{
position: relative;
width: 100%;
height: 117px;
left: 0px;
top: 0px;
background: #242E5E;
}
.newCity{
position: relative;
float:right;
width: 463px;
height: 40px;
top: 33px;
background: #FFFFFF;
border-radius: 10px;
margin-right:10px;
}
.weatherforecast{
    width:50%;
    height:200px;
    background-color: blue;
    color: #FFFFFF;
    text-align: center;
}
.weatherday{
    width:190px;
    vertical-align: middle;
    background-color: #FFFFFF;
}
h1{
    color: #FFFFFF;
}
</style>
