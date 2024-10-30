<script lang="ts" setup>
import { ref, computed } from 'vue'

let err = ref();
let inputValue = ref("");
let depositAmount = ref(0);
let renderDepositAmount = computed(() => {
    return ref(new Intl.NumberFormat('nb-NO', { style: 'currency', currency: 'NOK' }).format(depositAmount.value));
});

function handleDepositInput() {
    const depositValue = parseFloat(inputValue.value);
    if (!isNaN(depositValue)) {
        depositAmount.value = depositValue;
        deposit(depositValue);
    }
}

let userId = ref(null)
// how to get userID?

async function deposit(amount: number) {
    const depositAmount: number = amount;
    const response = await useFetch(
        () => `/api/users/${userId || -1}/wallet/deposit`,
        {
            method: 'POST',
            immediate: true,
            params: {
                value: depositAmount,
            },
        },
    )
    err.value = response?.error?.value
}
</script>

<template>
    <v-container>
        <v-row>
            <v-col>
                <v-text-field type="number" v-model="inputValue" placeholder="Amount to deposit..." />
                <v-btn @click="handleDepositInput">Deposit</v-btn>
            </v-col>
            <v-col>
                <p> Deposit amount: {{renderDepositAmount}}</p>
            </v-col>
        </v-row>
    </v-container>
</template>
