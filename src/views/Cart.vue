<template>
    <div class="page-cart">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Корзина покупок</h1>
            </div>
            <div class="column is-12 box">
                <table class="table is-fullwidth" v-if="cartTotalLength">
                    <thead>
                        <tr>
                            <th>Товар</th>
                            <th>Стоимость</th>
                            <th>Количество</th>
                            <th>Итого</th>
                            <th>Удалить</th>
                        </tr>
                    </thead>

                    <tbody id="select_txt">
                        <CartItem class="table table-sm"
                            v-for="item in cart.items"
                            v-bind:key="item.product.id"
                            v-bind:initialItem="item"
                            v-on:removeFromCart="removeFromCart" />
                &nbsp;
                <h2 class="subtitle">Итого</h2>
                <strong>{{ cartTotalPrice.toFixed(2) }} сом</strong>, {{ cartTotalLength }} шт.
                    </tbody>
                </table>
                
                <p v-else>У вас нет товаров в корзине...</p>
                
                <span class="buttons">
                    <button class="button is-success" @click="copyToClipboard('select_txt')">Нажмите чтобы скопировать данные!</button>
                </span>
            </div>
                 
            </div>
            <button class="button is-link is-aligned-center is-large" @click="getLinkWhastapp">Связаться с продавцом</button>    
                    
                
        </div>
            
    
</template>

<script>
import { toast } from 'bulma-toast'
import CartItem from '@/components/CartItem.vue'


export default {
    name: 'Cart',
    components: {
        CartItem
    },
    data() {
        return {
            cart: {
                items: []
            }
        }
    },
    mounted() {
        this.cart = this.$store.state.cart
    },
    methods: {
        removeFromCart(item) {
            this.cart.items = this.cart.items.filter(i => i.product.id !== item.product.id)
        }, 
        getLinkWhastapp(){
            window.location.href="https://api.whatsapp.com/send?phone=996705888080"
            return false
        },

        copyToClipboard(containerid) {
      let containerNode = document.getElementById(containerid);
      var textArea = document.createElement("textarea");
      textArea.style.position = "fixed";
      textArea.style.top = 0;
      textArea.style.left = 0;

      // Ensure it has a small width and height. Setting to 1px / 1em
      // doesn't work as this gives a negative w/h on some browsers.
      textArea.style.width = "2em";
      textArea.style.height = "2em";

      // We don't need padding, reducing the size if it does flash render.
      textArea.style.padding = 0;

      // Clean up any borders.
      textArea.style.border = "none";
      textArea.style.outline = "none";
      textArea.style.boxShadow = "none";

      // Avoid flash of white box if rendered for any reason.
      textArea.style.background = "transparent";

      textArea.value = containerNode.innerText;

      document.body.appendChild(textArea);
      textArea.focus();
      textArea.select();
      document.execCommand("copy");
      window.getSelection().removeAllRanges();
      document.body.removeChild(textArea);
      this.$store.commit('copyToClipboard')
      toast({message: 'Данные скопированы!',
        type: 'is-success',
        dismissible: true,
        pauseOnHover: true,
        duration: 2000,
        position: 'bottom-center'
        })

    },
    
    },
    computed: {
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        },
    }
        }
</script>