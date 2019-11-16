<template>
    <div class="container">
        <div style="margin: 10px;">
            <h1>Produtos</h1>
            <form
                method="post"
                @submit.prevent="storeProduct"
                style="margin: 20px;"
            >
                <div class="form-group">
                    <label for="">Nome</label>
                    <input 
                        type="text"
                        class="form-control"
                        aria-describedby="helpId"
                        placeholder=""
                        v-model="product.name"
                        required
                    >
                </div>

                <br>

                <div class="form-group">
                    <label for="">Quantidade</label>
                    <input 
                        type="number"
                        class="form-control"
                        aria-describedby="helpId"
                        placeholder=""
                        v-model="product.amount"
                        required
                    >
                </div>

                <br>

                <div class="form-group">
                    <label for="">Valor</label>
                    <input 
                        type="number"
                        class="form-control"
                        aria-describedby="helpId"
                        placeholder=""
                        v-model="product.value" 
                        required
                    >
                </div>

                <!-- TODO
                <div class="alert alert-danger" role="alert">
                    <strong>{{error}}</strong>
                </div> -->

                <br>

                <div class="form-group float-right">
                    <button
                        type="submit"
                        class="btn btn-dark btn-lg"
                    >
                        Cadastrar/Editar
                    </button>
                </div>
            </form>

            <table class="table table-dark">
                <thead>
                    <tr>
                        <th>Name</th>
                        <th>Amount</th>
                        <th>Price</th>
                        <th>Ações</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="product in products" v-bind:key="product.id">
                        <td scope="row">{{product.name}}</td>
                        <td>{{product.amount}}</td>
                        <td>{{product.value}}</td>
                        <td>
                            <button
                                type="button"
                                class="btn btn-primary btn-sm"
                                @click="showProduct(product.id)"
                            >
                                Editar
                            </button>
                            <button
                                type="button"
                                class="btn btn-danger btn-sm"
                                @click="deleteProduct(product.id)"
                            >
                                Remover
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
<style>
    button {
        margin: 10px;
    }
    body {
        background-color: black;
        color: white;
    }
    td, table, h1 {
        color: white;
        text-align: center;
    }
</style>
<script>
    import axios from 'axios';
    import 'bootstrap';
    import 'bootstrap/dist/css/bootstrap.min.css';
    export default {
        mounted() {
            this.loadProducts();
        },
        data: () => {
            return {
				error: "",
				productUrl: 'http://127.0.0.1:3333/product/',
                products: [],
                product: {
                    id: '',
                    name: '',
                    amount: '',
                    value: ''
                },
            }
        },
        methods: {
            loadProducts() {
                axios.get(this.productUrl).then(res => this.products = res.data);
            },
            storeProduct() {
                axios
                    .post(this.productUrl + this.product.id, this.product)
                    .then(res =>  {
                            if (res.data.error) {
                                this.error = res.data.error;
                            }
                            this.product = {};
                            this.loadProducts();
                        }
                    )
            },
            deleteProduct(id) {
                axios.delete(this.productUrl + id)
                    .then(res => {
                            if (res.data.success == true) {
                                this.loadProducts()
                            }
                        }
                    )
            },
            showProduct(id) {
                axios.get(this.productUrl + id).then(res => this.product = res.data)
            }
        },
    }
</script>