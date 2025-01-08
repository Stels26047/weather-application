<script>
    import axios from 'axios' 
    export default{
        data(){
            return {
                city: '',
                error: '',
                info: null
            }
        },
        computed:{
            cityName(){
                return "'" + this.city + "'"
            }
        },
        methods:{
            getWeather(){
                if(this.city.trim().length < 2){
                    this.error = "Нужно название более одного символа :)"
                    return false
                }

                this.error = "";

                axios.get(`http://api.weatherapi.com/v1/current.json?key=6170e44fefa74e0a88f183822250801&q=${this.city}&aqi=no`)
                .then((res) => (this.info = res.data.current.temp_c));
            }
        }
    }
</script>

<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{city == "" ? "вашем городе" : "'" + cityName + "'"}}</p>
        <input type="text" v-model="city" placeholder="Введите города">
        <button v-if="city != ''" @click="getWeather">Получить погоду</button>
        <button v-else disabled>Введите название города</button>
        <p class="error">{{error}}</p>
        <p v-show="info != null" style="margin-top: 2%;">{{ 
            "Температура на данный момент: " + info + ' °С' 
        }}</p>
        <snap>Название города должно быть на английском языке для корректной работы</snap>
    </div>
</template>

<style scoped>

    snap{
        display: block;
        margin-top: 3%;
    }

    .error{
        color:#d03939;
    }

    .wrapper{
        width: 900px;
        height: 500px;
        border-radius:  50px;
        background:  #1f0f24;
        text-align: center;
        color:#fff;
        margin-top: 5%;
        padding-top: 5%;
    }

    .wrapper h1{
        margin-top: 50px;
    }

    .wrapper input{
        margin-top: 30px;
        background: transparent;
        border: 0;
        border-bottom: 2px solid #110813;
        color: #fcfcfc;
        font-size: 14px;
        padding: 5px 8px;
        outline: none;
    }

    .wrapper input:focus{
        border-bottom-color: #6e2d7d;
    }

    .wrapper button:disabled{
        background:  #746027;
        cursor: not-allowed;
    }

    .wrapper button{
        background:  #e3bc4b;
        color:#fff;
        border-radius: 10px;
        border: 2px solid #b99935;
        padding: 10px 15px;
        margin-left: 20px;
        cursor: pointer;
        transition:  transform 500ms ease;
    }

    .wrapper button:hover{
        transform: scale(1,1) translateY(-5px);
    }
    
    p{
        font-size: 30px;
    }
</style>
