<template>
    <div class="product">
        <HeaderShayna />
        <!-- Breadcrumb Section Begin -->
        <div class="breacrumb-section">
            <div class="container">
                <div class="row">
                    <div class="col-lg-12">
                        <div class="breadcrumb-text product-more text-left">
                            <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                            <span>Detail</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Breadcrumb Section Begin -->

        <!-- Product Shop Section Begin -->
        <section class="product-shop spad page-details">
            <div class="container">
                <div class="row">
                    <div class="col-lg-12">
                        <div class="row">
                            <div class="col-lg-6">
                                <div class="product-pic-zoom">
                                    <img class="product-big-img" :src='default_img' alt="" />
                                </div>
                                <div class="product-thumbs" v-if="thumbs.length > 0">
                                    <carousel class="product-thumbs-track ps-slider" :nav='false' :dots='false'>
                                        <div class="pt" v-for="thumb in thumbs" :key="thumb.id" @click="changeImage(thumb.photo)" :class="thumb.photo == default_img ? 'active' : ''">
                                            <img :src="thumb.photo" alt="" />
                                        </div>
                                    </carousel>
                                </div>
                            </div>
                            <div class="col-lg-6">
                                <div class="product-details text-left">
                                    <div class="pd-title">
                                        <span>{{ detailProduct.type }}</span>
                                        <h3>{{ detailProduct.name }}</h3>
                                    </div>
                                    <div class="pd-desc">
                                        <p> {{ detailProduct.description }} </p>
                                        <h4>${{ detailProduct.price }}</h4>
                                    </div>
                                    <div class="quantity">
                                        <router-link to="/cart">
                                            <a href="#" class="primary-btn pd-cart">Add To Cart</a>
                                        </router-link>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <!-- Product Shop Section End -->
        <RelatedShayna/>
        <FooterShayna />
    </div>
</template>

<script>
    // @ is an alias to /src
    import HeaderShayna from '@/components/HeaderShayna.vue'
    import FooterShayna from '@/components/FooterShayna.vue'
    import RelatedShayna from '@/components/RelatedShayna.vue'
    import carousel from 'vue-owl-carousel'
    import axios from 'axios'

    export default {
        name: 'Product',
        components: {
            HeaderShayna,
            FooterShayna,
            RelatedShayna,
            carousel
        },
        data() {
            return {
                detailProduct: [],
                default_img: '',
                thumbs: [],
            }
        },
        methods: {
            changeImage(imgURL) {
                this.default_img = imgURL;
            },
            setDataPicture(data) {
                this.detailProduct = data;
                // asumsi foto produk pertama upload adalah untuk thumbnail utama
                this.default_img = data.galleries[0].photo;
                this.thumbs = data.galleries;
            },
        },
        mounted() {
            axios
                .get('http://larashop.site/api/products', {
                    params: {
                        id: this.$route.params.id
                    }
                })
                .then(res => this.setDataPicture(res.data.data))
                .catch(err => console.log(err));
        },
    }
</script>

<style scoped>
    .product-thumbs .pt {
        margin-right: 10px;
    }
</style>