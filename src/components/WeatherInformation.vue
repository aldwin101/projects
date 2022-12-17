<template>
    <div class="container">
        <div class="inner-container">
            <input type="text" id="" placeholder="enter location" v-model="location">
            <button @click="GetWeatherData()">Get Data</button>
        </div>
        <h1>{{city}}</h1>
        <h3>{{region}}</h3>
        <h3>{{country}}</h3>
        <p>{{time}}</p>
        <p>{{timezone}}</p>
        <p>{{temperature}}</p>
        <p>{{feels_like}}</p>
        <p>{{condition_text}}</p>
        <img :src="icon" alt="">
        <p>{{forecast}}</p>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        data() {
            return {
                location: "",
                city: "",
                region: "",
                country: "",
                time: "",
                timezone: "",
                temperature: "",
                feels_like: "",
                condition_text: "",
                icon: "",
                condition_icon: "",

                forecast: "",
                
            }
        },
        methods: {
            // funtion call
            GetWeatherData() {
                // axios call for current weather
                axios.request({
                    url: 'https://api.weatherapi.com/v1/current.json',
                    params: {
                        key: process.env.VUE_APP_API_KEY,
                        q: this.location
                    },
                    methods: 'GET',
                }).then((response) => {
                    // console.log(response);
                    this.city = response.data.location.name
                    this.region = response.data.location.region
                    this.country = response.data.location.country
                    this.time = response.data.location.localtime
                    this.timezone = "Timezone: " + response.data.location.tz_id
                    this.temperature = "Temperature: " + response.data.current.temp_c + "°"
                    this.feels_like = "Feels like: " + response.data.current.feelslike_c + "°"
                    this.condition_text = response.data.current.condition.text
                    this.condition_icon = response.data.current.condition.icon
                    this.icon = this.condition_icon
                    axios.request({
                        url: 'https://api.weatherapi.com/v1/forecast.json',
                        params: {
                            key: process.env.VUE_APP_API_KEY,
                            q: this.location 
                        },
                        method: 'GET',
                    }).then ((response) => {
                        this.forecast = response.data.forecast.forecastday

                    }).catch ((error) => {
                        console.log(error);
                    })
        
                }).catch ((error) => {
                    console.log(error);

                })
            }
        },
    }
</script>

<style lang="scss" scoped>
.container{
    width: 100%;

    .inner-container{
        width: 50%;
        
    }
}
</style>