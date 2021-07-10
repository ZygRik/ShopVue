<template>
    <div class="page-checkout">
        <div class="column is-multiline">
            <div class="column is-12">
                <h1 class="title">Оформление Вашего заказа:</h1>
            </div>

                <div class="column is-12 box">
                    <table class="table is-fullwidth">
                        <thead>
                            <tr>
                                <th>Товар</th>
                                <th>Стоимость</th>
                                <th>Количество</th>
                                <th>Итого</th>
                            </tr>
                        </thead>

                        <tbody>
                            <tr 
                                v-for="item in cart.items"
                                v-bind:key="item.product.key"
                            >
                                <td>{{ item.product.name }}</td>
                                <td>{{ item.product.price }}</td>
                                <td>{{ item.quantity }}</td>
                                <td>{{ getItemTotal(item).toFixed(2) }}</td>
                            </tr>
                        </tbody>

                        <tfoot>
                            <td colspan="2">Итого:</td>
                            <td>{{ cartTotalLength }}</td>
                            <td>{{ cartTotalPrice.toFixed(2)}} сом</td>
                        </tfoot>
                    </table>
                </div>

                <div class="column is-12 box">
                    <!-- <h2 class="subtitle has-text-centered">Информация по доставке:</h2> -->
                    <!-- <p class="has-text-grey mb-4 has-text-centered">* необходимые поля</p> -->
                    
                    <div class="columns is-multiline is-centered">
                        <div class="column is-6">
                            <!-- <div class="field">
                                <label>* Имя:</label>
                                <div class="control">
                                    <input type="text" class="input" v-model="first_name" aria-required="true">
                                </div>
                            </div> -->

                            <!-- <div class="field">
                                <label>Фамилия:</label>
                                <div class="control">
                                    <input type="text" class="input" v-model="last_name" aria-required="true">
                                </div>
                            </div> -->
                            
                             <!-- <div class="field">
                                <label>* Номер телефона:</label>
                                <div class="control">
                                    <input type="tel" id="phone" class="input" v-model="phone" aria-required="true" pattern="[0-9]{3}-[0-9]{2}-[0-9]{2}-[0-9]{2}" placeholder="введите Ваш номер телефона БЕЗ префикса +996">
                                </div>
                            </div> -->

                             <!-- <div class="field">
                                <label>* Адрес:</label>
                                <div class="control">
                                    <input type="text" class="input" v-model="address" aria-required="true" placeholder="Введите точный адрес доставки">
                                </div>
                            </div> -->

                             <!-- <div class="field">
                                <label>электронная почта:</label>
                                <div class="control">
                                    <input type="text" class="input" v-model="last_name" aria-required="false">
                                </div>
                            </div> -->
                                <!-- <div class="notification is-danger mt-4" v-if="errors.length">
                                    <p v-for="error in errors" v-bind:key="error">{{error}}</p>
                                </div> -->
                             

                                <div id="card-element" class="mb-5 has-text-centered">

                                    <template v-if="cartTotalLength">
                                        

                                        <button class="button is-link is-aligned-center is-large" @click="getLinkWhastapp">Связаться с продавцом</button>    
                                    </template>


                                </div>
                        </div>
                    </div> 

                </div>

        </div>
    </div>
</template>

<script>
import axios from 'axios'


    export default {
        name: 'Checkout',
        data(){
            return {
                cart: {
                    items: []
                },
                first_name: '',
                last_name: '',
                phone: '',
                address: '',
                errors: []
            }
        },
        mounted(){
            document.title = 'Оформление заказа|DeJaVue'

            this.cart = this.$store.state.cart
        },
        methods: {
            getItemTotal(item) {
                return item.quantity * item.product.price
            }, 
            submitForm(){
                this.errors = []
            if (this.first_name === '') {
                this.errors.push('Не указано имя!')
            }
            /*if (this.last_name === '') {
                this.errors.push('Не указана фамилия!')
            }*/
            if (this.phone === '') {
                this.errors.push('Не указан номер телефона!')
            }
            if (this.address === '') {
                this.errors.push('Не указан адрес доставки!')
            }
            
            },
        },
        computed: {
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        },
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
            }
        },

    }
</script>

<style scoped>

</style>