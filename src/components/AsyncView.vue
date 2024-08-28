<template>
    <div class="flex flex-col flex-1 items-center">
        <!-- Banner -->
        <div v-if="route.query.preview" class="text-white bg-weather-secondary w-full text-center">
            <p>You are currently previewing this city, click the "+"
                icon to start tracking this city.</p>
        </div>

        <!-- WeatherOverview -->
        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
            <p class="text-sm mb-12">
                {{
                    new Date(weatherData.dt).toLocaleDateString("en-us",
                        {
                            weekday: "short",
                            day: "2-digit",
                            month: "long",
                        }
                    )
                }}
                {{
                    new Date(weatherData.dt).toLocaleTimeString(
                        "en-us",
                        {
                            timeStyle: "short",
                        }
                    )
                }}
            </p>
            <p class="text-8xl mb-8">
                {{ Math.round(weatherData.main.temp) }}&deg;
            </p>
            <p>
                Feels like
                {{ Math.round(weatherData.main.feels_like) }}&deg;
            </p>
            <p class="capitalize">
                {{ weatherData.weather[0].description }}
            </p>
            <img class="w-[150px] h-auto" :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`
                " alt="" />
        </div>

        <hr class="border-white border-opacity-10 border w-full" />


    </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const getWeatherData = async () => {
    try {
        // Fetch weather data
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.lng}&appid=470ce6c6e497c1abe271144f31b5c98c&units=imperial`);
        const weatherData = response.data;

        // Log the entire response to verify structure
        // console.log('Weather Data:', weatherData);

        // Ensure `dt` is defined before accessing it
        if (weatherData && weatherData.dt) {
            console.log('Date and Time:', weatherData.dt);
        } else {
            console.error('`dt` is undefined in the weather data.');
        }

        // Calculate the current local time
        const localOffset = new Date().getTimezoneOffset() * 60000;
        const utc = weatherData.dt * 1000 + localOffset;;
        const localTime = utc - localOffset + (weatherData.timezone * 1000);
        weatherData.currentTime = new Date(localTime).toLocaleString();



        return weatherData;

    } catch (err) {
        console.log(err);
    }
};


const weatherData = await getWeatherData();
//console.log(weatherData);

</script>