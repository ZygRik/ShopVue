<template>
  <div class="card">
       <div class="card-content">

    <div class="content">
        <div class="title has-text-grey">
            <h1 class="has-text-centered has-text-black-ter">Наши контакты:</h1>
            <p is-size-6>
                Адрес: г. Бишкек, ул. Горького 1Г, 
                Торговый центр "Таш-Рабат", 2-й этаж,
                бутик №   , <strong class="has-text-danger" id="has-text-justified">тел./whatsapp: +996 705 888080</strong>
            </p>
        </div>

        <div class="columns is-centered">
            <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2255.2712408995108!2d74.61616348217194!3d42.85754182284008!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x389eb649ed4a0ae7%3A0xe210637f4f9f53dd!2z0KLQsNGI0YDQsNCx0LDRgiDQodC-0L7QtNCwINKv0LnSrw!5e0!3m2!1sen!2skg!4v1625342223918!5m2!1sen!2skg" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy"></iframe>
        </div>
      
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'


    export default {
        name: 'Contacts',

        data(){
            return {
            username: '',
                password: '',
                errors: []
            }
        },
        mounted(){
            document.title = 'Контакты | Онлайн магазин'
        },
        methods: {
            async submitForm(){
                axios.defaults.headers.common["Authorization"] = ""

                localStorage.removeItem("token")

                const formData = {
                    username: this.username,
                    password: this.password
                }
                await axios
                .post("/api/v1/token/contacts/", formData)
                .then(response => {
                    const token = response.data.auth_token
                    this.$store.commit('setToken', token)
                    
                    axios.defaults.headers.common["Authorization"] = "Token " + token
                    localStorage.setItem("token", token)
                    const toPath = this.$route.query.to || '/cart'
                    this.$router.push(toPath)
                })
                .catch(error => {
                    if (error.response) {
                        for (const property in error.response.data) {
                            this.errors.push(`${property}: ${error.response.data[property]}`)
                        }
                    } else {
                        this.errors.push('Что-то пошло не так! Повторите снова!')
                        
                        console.log(JSON.stringify(error))
                    }
                })
        }
    }
}
</script>