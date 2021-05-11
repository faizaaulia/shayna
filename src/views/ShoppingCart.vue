<template>
    <div>
        <HeaderShayna />
        <!-- Breadcrumb Section Begin -->
        <div class="breacrumb-section">
            <div class="container">
                <div class="row">
                    <div class="col-lg-12">
                        <div class="breadcrumb-text product-more text-left">
                            <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                            <span>Shopping Cart</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Breadcrumb Section End -->

        <!-- Shopping Cart Section Begin -->
        <section class="shopping-cart spad">
            <div class="container">
                <div class="row">
                    <div class="col-lg-8">
                        <div class="row">
                            <div class="col-lg-12">
                                <div class="cart-table">
                                    <table>
                                        <thead>
                                            <tr>
                                                <th>Image</th>
                                                <th class="p-name text-center">Product Name</th>
                                                <th>Price</th>
                                                <th>Action</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr v-for="cart in userCart" :key="cart.id">
                                                <td class="cart-pic first-row">
                                                    <img :src="cart.photo" />
                                                </td>
                                                <td class="cart-title first-row text-center">
                                                    <h5>{{ cart.name }}</h5>
                                                </td>
                                                <td class="p-price first-row">${{ cart.price }}</td>
                                                <td class="delete-item">
                                                    <a href="#" @click="removeItem(userCart.index)">
                                                        <i class="material-icons">close</i>
                                                    </a>
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                            <div class="col-lg-8 text-left">
                                <h4 class="mb-4">
                                    Informasi Pembeli:
                                </h4>
                                <div class="user-checkout">
                                    <form>
                                        <div class="form-group">
                                            <label for="namaLengkap">Nama lengkap</label>
                                            <input type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama" v-model="userCheckout.name">
                                        </div>
                                        <div class="form-group">
                                            <label for="namaLengkap">Email Address</label>
                                            <input type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email" v-model="userCheckout.email">
                                        </div>
                                        <div class="form-group">
                                            <label for="namaLengkap">No. HP</label>
                                            <input type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP" v-model="userCheckout.phone">
                                        </div>
                                        <div class="form-group">
                                            <label for="alamatLengkap">Alamat Lengkap</label>
                                            <textarea class="form-control" id="alamatLengkap" rows="3" v-model="userCheckout.address"></textarea>
                                        </div>
                                    </form>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-lg-4">
                        <div class="row">
                            <div class="col-lg-12">
                                <div class="proceed-checkout">
                                    <ul class="text-left">
                                        <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                        <li class="subtotal mt-3">Subtotal <span>${{ totalCart }}</span></li>
                                        <li class="subtotal mt-3">Pajak <span>10%</span></li>
                                        <li class="subtotal mt-3">Total Biaya <span>${{ totalCart * (1+0.1) }}</span></li>
                                        <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                        <li class="subtotal mt-3">No. Rekening <span>1234 8765 0987</span></li>
                                        <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                    </ul>
                                    <!-- <router-link to="/success" @click="checkout()"> -->
                                        <a href="#" class="proceed-btn" @click="checkout()">I ALREADY PAID</a>
                                    <!-- </router-link> -->
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <!-- Shopping Cart Section End -->
    </div>
</template>

<script>
    import HeaderShayna from '@/components/HeaderShayna'
    import axios from 'axios';

    export default {
        name: 'Cart',
        components: {
            HeaderShayna
        },
        data() {
            return {
                userCart: [],
                totalCart: 0,
                userCheckout: {
                    name: '',
                    email: '',
                    phone: '',
                    address: '',
                }
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
        methods: {
            removeItem(idx) {
                // TODO: fix missmatch item removing
                this.userCart.splice(idx, 1);
                // save newest cart
                const parsed = JSON.stringify(this.userCart);
                localStorage.setItem('userCart', parsed);
                this.totalCart = this.userCart.reduce((acc, item) => acc + item.price, 0)
            },
            checkout() {
                let trx_details = this.userCart.map((product) => {
                    return product.id
                })
                
                let checkoutData = {
                    'name': this.userCheckout.name,
                    'email': this.userCheckout.email,
                    'phone': this.userCheckout.phone,
                    'address': this.userCheckout.address,
                    'total': this.totalCart,
                    'status': "PENDING",
                    'transaction_details': trx_details
                }

                axios.post('http://127.0.0.1:8000/api/checkout', checkoutData)
                    .then(() => this.$router.push('/success'))
                    .catch(e => console.log(e))
            }
        },
    }
</script>