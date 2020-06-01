<template>
    <div>
        <!-- Women Banner Section Begin -->
        <section class="women-banner spad">
            <div class="container-fluid">
                <div class="row">
                    <div class="col-lg-12 mt-5" v-if="products.length > 0">
                        <carousel class="product-slider" :nav='false' :item='3' :autoplay='true' :dots='false'>
                            <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
                                <div class="pi-pic">
                                    <img src="img/products/women-2.jpg" alt="" />
                                    <ul>
                                        <li class="w-icon active">
                                            <a href="#"><i class="icon_bag_alt"></i></a>
                                        </li>
                                        <li class="quick-view">
                                            <router-link to="/product">+ Quick View</router-link>
                                        </li>
                                    </ul>
                                </div>
                                <div class="pi-text">
                                    <div class="catagory-name">{{ itemProduct.type }}</div>
                                    <router-link to="/product">
                                        <h5>{{ itemProduct.name }}</h5>
                                    </router-link>
                                    <div class="product-price">
                                        ${{ itemProduct.price }}
                                    </div>
                                </div>
                            </div>
                        </carousel>
                    </div>
                    <div class="col-lg-12 mt-5" v-else>
                        <p>Produk belum tersedia untuk saat ini</p>
                    </div>
                </div>
            </div>
        </section>
        <!-- Women Banner Section End -->
    </div>
</template>

<script>
import carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
    name: 'BannerShayna',
    components: {
        carousel
    },
    data() {
        return {
            products: []
        };
    },
    mounted() {
        axios
            .get('http://127.0.0.1:8000/api/products')
            .then(res => (this.products = res.data.data.data))
            .catch(err => console.log(err));
    }
}
</script>

<style scoped>
    .product-item {
        margin-right: 25px;
    }
</style>