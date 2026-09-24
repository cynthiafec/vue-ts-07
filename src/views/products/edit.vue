<script setup lang="ts">

// import ref dan onMounted dari vue
import { ref, onMounted } from "vue";

// import useRoute dan useRouter dari vue-router
import { useRoute, useRouter } from "vue-router";

// import Api dari folder api
import Api from "../../api";

// Interface Errors
interface Errors {
    name?: string[];
    description?: string[];
    price?: string[];
    stock?: string[];
}

// State untuk form
const name = ref("");
const description = ref("");
const price = ref("");
const stock = ref("");

// State errors
const errors = ref<Errors>({});

// initiate route dan router
const route = useRoute();
const router = useRouter();


// Fetch product details
const fetchDetailProduct = async () => {
    try {

        // ambil data semua product
        const response = await Api.get("/api/product");

        // ambil data product
        const data = response.data?.data;

        // cek apakah data menggunakan pagination
        const products = Array.isArray(data)
            ? data
            : data?.data || [];

        // cari product berdasarkan id
        const product = products.find(
            (item: any) => item.id == route.params.id
        );

        // jika product ditemukan
        if (product) {
            name.value = product.name ?? "";
            description.value = product.description ?? "";
            price.value = product.price ?? "";
            stock.value = product.stock ?? "";
        }

    } catch (error) {

        // log error
        console.error("Error fetching product:", error);
    }
};


// run hook "onMounted"
onMounted(() => {

    // call method "fetchDetailProduct"
    fetchDetailProduct();

});


// Handle form submission
const updateProduct = async () => {

    // reset error terlebih dahulu
    errors.value = {};

    // validasi Name
    if (!name.value.trim()) {
        errors.value.name = ["The name field is required."];
    }

    // validasi Description
    if (!description.value.trim()) {
        errors.value.description = ["The description field is required."];
    }

    // validasi Price
    if (!price.value) {
        errors.value.price = ["The price field is required."];
    }

    // validasi Stock
    if (!stock.value) {
        errors.value.stock = ["The stock field is required."];
    }

    // jika ada error, jangan kirim data
    if (Object.keys(errors.value).length > 0) {
        return;
    }


    // initiate form data
    const formData = new FormData();

    // append data ke form data
    formData.append("name", name.value);
    formData.append("description", description.value);
    formData.append("price", price.value);
    formData.append("stock", stock.value);

    // method PUT
    formData.append("_method", "PUT");


    try {

        // kirim data ke API
        await Api.post(
            `/api/product/${route.params.id}`,
            formData
        );

        // kembali ke halaman products
        router.push("/products");

    } catch (error: any) {

        // tampilkan error dari server
        console.error(
            "ERROR DARI SERVER:",
            error.response?.data
        );

        // simpan error dari Laravel
        errors.value =
            error.response?.data?.errors || {};

    }
};
</script>


<template>
    <div class="container mt-5">

        <div class="row">

            <div class="col-md-12">

                <div class="card border-0 rounded-3 shadow">

                    <div class="card-body">

                        <form @submit.prevent="updateProduct">


                            <!-- NAME -->
                            <div class="mb-3">

                                <label class="form-label fw-bold">
                                    Name
                                </label>

                                <input
                                    type="text"
                                    v-model="name"
                                    @input="errors.name = undefined"
                                    class="form-control"
                                    placeholder="Name Product"
                                />

                                <div
                                    v-if="errors.name"
                                    class="alert alert-danger mt-2"
                                >
                                    {{ errors.name[0] }}
                                </div>

                            </div>


                            <!-- DESCRIPTION -->
                            <div class="mb-3">

                                <label class="form-label fw-bold">
                                    Description
                                </label>

                                <textarea
                                    v-model="description"
                                    @input="errors.description = undefined"
                                    class="form-control"
                                    rows="5"
                                    placeholder="Description Product"
                                ></textarea>

                                <div
                                    v-if="errors.description"
                                    class="alert alert-danger mt-2"
                                >
                                    {{ errors.description[0] }}
                                </div>

                            </div>


                            <!-- PRICE & STOCK -->
                            <div class="row">


                                <!-- PRICE -->
                                <div class="col-md-6">

                                    <div class="mb-3">

                                        <label class="form-label fw-bold">
                                            Price
                                        </label>

                                        <input
                                            type="number"
                                            v-model="price"
                                            @input="errors.price = undefined"
                                            class="form-control"
                                            placeholder="Price Product"
                                        />

                                        <div
                                            v-if="errors.price"
                                            class="alert alert-danger mt-2"
                                        >
                                            {{ errors.price[0] }}
                                        </div>

                                    </div>

                                </div>


                                <!-- STOCK -->
                                <div class="col-md-6">

                                    <div class="mb-3">

                                        <label class="form-label fw-bold">
                                            Stock
                                        </label>

                                        <input
                                            type="number"
                                            v-model="stock"
                                            @input="errors.stock = undefined"
                                            class="form-control"
                                            placeholder="Stock Product"
                                        />

                                        <div
                                            v-if="errors.stock"
                                            class="alert alert-danger mt-2"
                                        >
                                            {{ errors.stock[0] }}
                                        </div>

                                    </div>

                                </div>

                            </div>


                            <!-- BUTTON UPDATE -->
                            <button
                                type="submit"
                                class="btn btn-md btn-primary rounded-5 shadow border-0"
                            >
                                Update
                            </button>


                        </form>

                    </div>

                </div>

            </div>

        </div>

    </div>
</template>