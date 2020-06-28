<template>
    <div class="container">
        <div id="app"
             :class="typeof weather.main != 'undefined' && weather.main.temp > 293 ? 'warm' : ''">
            <main>
                <div class="search-bar">
                    <input type="text"
                           placeholder="Search..."
                           v-model="query"
                           @keypress.enter="getWeather"
                    >
                </div>

                <div class="weather-info" v-if="typeof weather.main != 'undefined'">
                    <div>
                        <div class="location">
                            {{`${weather.name}, ${weather.sys.country}`}}
                        </div>
                    </div>

                    <div class="temp-wrap">
                        <div class="temp">
                            {{Math.round(1.8 * Math.round(weather.main.temp - 273.15) + 32) }}°f
                        </div>
                    </div>

                    <div>
                        <div class="desc">
                            {{weather.weather[0].description}}
                        </div>
                    </div>
                </div>

                <div class="blankCity" v-else>
                    <!-- make prettier -->
                    <h2>Please enter a city</h2>
                </div>
            </main>
        </div>
    </div>
</template>

<script>

    export default {
        name: 'App',
        data() {
            return {
                query: '',
                api_key: process.env.VUE_APP_API_KEY,
                api_url: process.env.VUE_APP_API_URL,
                weather: []
                // api_key: process.env.API_KEY
            }
        },
        methods: {
            getWeather() {
                fetch(`${this.api_url}weather?q=${this.query}&APPID=${this.api_key}`)
                    .then(res => {
                        return res.json()
                    })
                    .then(this.setWeather)
            },
            setWeather(results) {
                this.weather = results
            }
        },
        mounted() {
            console.log(process.env.API_KEY)
        }
    }
</script>

<style>

    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        display: flex;
        justify-content: center;
        background-color: rgba(22, 36, 54, 0.46);
    }

    #app {
        font-family: Avenir, 'sans-serif';
        text-align: center;
        color: #2c3e50;
        background-image: linear-gradient(rgba(202, 254, 255, 0.65), rgba(175, 238, 238, 0.65), rgba(5, 166, 255, 0.65));
        min-height: 100vh;
        /*transition: height 2s ease-in;*/
        display: flex;
        align-items: center;
    }

    #app.warm {
        background-image: linear-gradient(rgba(255, 255, 224, 0.65), rgba(255, 165, 0, 0.65), rgba(255, 0, 0, 0.65));
    }

    .search-bar {
        width: 500px;
        padding-top: 60px;
    }

    .search-bar input {
        width: 75%;
        padding: 20px 0;
        text-align: center;
        border-radius: 50px;
        font-size: 1em;
        color: #313131;
        /*font-size: 20px;*/
        /*appearance: none;*/
        border: none;
    }

    .search-bar input:focus {
        outline: none;
    }

    .weather-info {
        margin-top: 30px;
    }

    .blankCity {
        margin-top: 20px;
    }

    .temp-wrap {
        display: flex;
        justify-content: center;
    }

    .temp {
        /*width:*/
        margin: 15px 0;
        color: #FFF;
        font-size: 102px;
        font-weight: 900;
        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.25);
        width: 50%;
        border-radius: 15px;
    }

    .location {
        color: white;
        font-weight: 600;
        font-size: 35px;
        letter-spacing: 1px;
    }

    .desc {
        font-weight: bolder;
        font-size: 15px;
        color: #313131;
        letter-spacing: 2px;
    }
</style>
