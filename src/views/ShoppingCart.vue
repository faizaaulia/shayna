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
                                        <tbody v-if="userCart.length > 0">
                                            <tr v-for="item in userCart" :key="item.id">
                                                <td class="cart-pic first-row">
                                                    <img :src="item.photo" class="img-cart" />
                                                </td>
                                                <td class="cart-title first-row text-center">
                                                    <h5>{{ item.name }}</h5>
                                                </td>
                                                <td class="p-price first-row">${{ item.price }}</td>
                                                <td class="delete-item">
                                                    <a href="#" @click="removeItem(item.id)"><i class="material-icons">close</i></a>
                                                </td>
                                            </tr>
                                        </tbody>
                                        <tbody v-else>
                                            <tr>
                                                <td colspan="4">Keranjang Masih Kosong</td>
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
                                            <input type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama" v-model="custInfo.name">
                                        </div>
                                        <div class="form-group">
                                            <label for="namaLengkap">Email Address</label>
                                            <input type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email" v-model="custInfo.email">
                                        </div>
                                        <div class="form-group">
                                            <label for="namaLengkap">No. HP</label>
                                            <input type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP" v-model="custInfo.phone">
                                        </div>
                                        <div class="form-group">
                                            <label for="alamatLengkap">Alamat Lengkap</label>
                                            <textarea class="form-control" id="alamatLengkap" rows="3" v-model="custInfo.address"></textarea>
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
                                        <li class="subtotal mt-3">Subtotal <span>${{ totalPrice }}.00</span></li>
                                        <li class="subtotal mt-3">Pajak <span>10% = ${{ taxes }}</span></li>
                                        <li class="subtotal mt-3">Total Biaya <span>${{ totalPay }}</span></li>
                                        <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                        <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                        <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                    </ul>
                                    <!-- <router-link to="/success"> -->
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
    import axios from 'axios'

    export default {
        name: 'Cart',
        components: {
            HeaderShayna
        },
        data() {
            return {
                userCart: [],
                custInfo: {
                    name: '',
                    email: '',
                    phone: '',
                    address: ''
                }
            }
        },
        methods: {
            removeItem(itemId) {
                let items = JSON.parse(localStorage.getItem('userCart'));
                let item = items.map(item => item.id); // iterate for each item id
                let index = item.findIndex(id => id == itemId); // find item where id == itemId
                this.userCart.splice(index, 1); // delete item with id index
                // save newes cart
                const parsed = JSON.stringify(this.userCart);
                localStorage.setItem('userCart', parsed);
            },
            checkout() {
                //get each product id, return with array
                let productIds = this.userCart.map(function(product) {
                    return product.id;
                });

                let checkoutData = {
                    'name': this.custInfo.name,
                    'email': this.custInfo.email,
                    'phone': this.custInfo.phone,
                    'address': this.custInfo.address,
                    'total': parseFloat(this.totalPay),
                    'status': 'PENDING',
                    'transaction_details': productIds
                };

                axios.post('http://larashop.site/api/checkout', checkoutData)
                     .then((response) => {
                         this.$router.push('success'); // move to success page
                         if (response.status)
                            localStorage.removeItem('userCart');
                     })
                     .catch(err => console.log(err));
            }
        },
        mounted() {
            if (localStorage.getItem('userCart')) {
                try {
                    this.userCart = JSON.parse(localStorage.getItem('userCart'));
                } catch (error) {
                    localStorage.removeItem('userCart');
                }
            }
        },
        computed: {
            totalPrice() {
                return this.userCart.reduce(function(items, data) {
                    return items + data.price;
                }, 0);
            },
            taxes() {
                return this.totalPrice * 0.1;
            },
            totalPay() {
                return this.taxes + this.totalPrice;
            }
        }
    }
</script>

<style scoped>
    .img-cart {
        width: 120px;
        height: 120px;
    }
</style>