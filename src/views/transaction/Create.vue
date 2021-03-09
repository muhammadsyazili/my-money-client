<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8">
                <router-link
                    :to="{name: 'transaction.index'}"
                    class="btn btn-primary btn-sm rounded ahadow mb-3"
                >Back</router-link>

                <div class="card rounded shadow">
                    <div class="card-header">
                        Create Transaction
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="store()">
                            <div class="mb-3">
                                <label for="" class="form-label">Title</label>
                                <input type="text" class="form-control is-invalid" v-model="transaction.title">
                                <div v-if="validation.title" class="invalid-feedback">
                                    {{validation.title[0]}}
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="" class="form-label">Amount</label>
                                <input type="number" class="form-control is-invalid" v-model="transaction.amount">
                                <div v-if="validation.amount" class="invalid-feedback">
                                    {{validation.amount[0]}}
                                </div>
                            </div>
                            <div class="mb-3">
                                <div class="form-check form-check-inline">
                                <input class="form-check-input" type="radio" id="expense" value="expense" checked v-model="transaction.type">
                                <label class="form-check-label" for="expense">Expense</label>
                                </div>
                                <div class="form-check form-check-inline">
                                <input class="form-check-input" type="radio" id="revenue" value="revenue" v-model="transaction.type">
                                <label class="form-check-label" for="revenue">Revenue</label>
                                </div>
                            </div>
                            <input type="hidden" v-model="transaction.user_id" value="1">
                            <div v-if="validation.user_id" class="invalid-feedback">
                                {{validation.user_id[0]}}
                            </div>
                            <button class="btn btn-outline-primary">Submit</button>
                        </form>
                    </div>
                </div>
                <!-- <div></div> -->
            </div>
        </div>  
    </div>
</template>

<script>
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import { axios } from "axios";
export default {
    setup() {
        // data binding
        const transaction = reactive({
            user_id: '',
            title: '',
            amount: '',
            type: '',
        });

        const validation = ref([]);

        const router = useRouter();

        function store() {
            axios.post(
                'http://127.0.0.1:8000/api/transaction',
                transaction
            )
            .then(() => {
                router.push({
                    name: 'transaction.index'
                });
            })
            .catch((err) => {
                validation.value = err.response.data
            });
        }

        return {
            transaction,
            validation,
            router,
            store
        }
    }
}
</script>