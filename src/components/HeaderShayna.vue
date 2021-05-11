<template>
    <div>
        <!-- Header Section Begin -->
        <header class="header-section">
            <div class="header-top">
                <div class="container">
                    <div class="ht-left">
                        <div class="mail-service">
                            <i class=" fa fa-envelope"></i> hello.shayna@gmail.com
                        </div>
                        <div class="phone-service">
                            <i class=" fa fa-phone"></i> +628 22081996
                        </div>
                    </div>
                </div>
            </div>
            <div class="container">
                <div class="inner-header">
                    <div class="row">
                        <div class="col-lg-2 col-md-2">
                            <div class="logo">
                                <router-link to="/">
                                    <img src="img/logo_website_shayna.png" alt="" />
                                </router-link>
                            </div>
                        </div>
                        <div class="col-lg-7 col-md-7"></div>
                        <div class="col-lg-3 text-right col-md-3">
                            <ul class="nav-right">
                                <li class="cart-icon">
                                    Keranjang Belanja &nbsp;
                                    <a href="#">
                                        <i class="icon_bag_alt ml-2"></i>
                                        <span>{{ userCart.length }}</span>
                                    </a>
                                    <div class="cart-hover">
                                        <div class="select-items">
                                            <table>
                                                <tbody v-if="userCart.length > 0">
                                                    <tr v-for="cart in userCart" :key="cart.id">
                                                        <td class="si-pic">
                                                            <img class="photo-item" :src="cart.photo" alt="" />
                                                        </td>
                                                        <td class="si-text">
                                                            <div class="product-selected">
                                                                <p>${{ cart.price }} x 1</p>
                                                                <h6>{{ cart.name }}</h6>
                                                            </div>
                                                        </td>
                                                        <td class="si-close" @click="removeItem(cart.index)">
                                                            <i class="ti-close"></i>
                                                        </td>
                                                    </tr>
                                                </tbody>
                                                <tbody v-else>
                                                    <tr>Keranjang Masih Kosong</tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <div class="select-total">
                                            <span>total:</span>
                                            <h5>${{ totalCart }}</h5>
                                        </div>
                                        <div class="select-button">
                                            <a href="#" class="primary-btn view-card">VIEW CART</a>
                                            <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                                        </div>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </header>
        <!-- Header End -->
    </div>
</template>

<script>
export default {
    name: 'HeaderShayna',
    data() {
        return {
            userCart: [],
            totalCart: 0
        }
    },
    methods: {
        removeItem(idx) {
            // TODO: fix missmatch item removing
            this.userCart.splice(idx, 1);
            // save newest cart
            const parsed = JSON.stringify(this.userCart);
            localStorage.setItem('userCart', parsed);
            this.totalCart = this.userCart.reduce((acc, item) => acc + item.price, 0)
        }
    },
    mounted() {
        if (localStorage.getItem('userCart')) {
            try {
                this.userCart = JSON.parse(localStorage.getItem('userCart'));
                this.totalCart = this.userCart.reduce((acc, item) => acc + item.price, 0)
            } catch (error) {
                localStorage.removeItem('userCart');
            }
        }
    },
}
</script>

<style scoped>
    .photo-item {
        width: 80px;
        height: 80px;
    }
</style>