<script lang="ts" setup>
import { ref, computed } from 'vue'

let err = ref()
let depositAmount = ref()
let currentAmount = ref(0)
let userID = ref() // i have no idea how to get userID
let deposited: boolean = false

let renderDepositAmount = computed(() => {
	return ref(
		new Intl.NumberFormat('nb-NO', {
			style: 'currency',
			currency: 'NOK',
		}).format(depositAmount.value)
	)
})


function handleDepositInput() {
    let startingAmount = getAmount(userID.value)
    updateAmount(userID.value, startingAmount + depositAmount.value)
    console.log("Deposited: ", depositAmount.value, " into ", userID.value, "'s account.")
}

async function getAmount(userID: number): number {
	try {
		const response = await fetch(`/api/users/${userID}/wallet/deposit`)
		currentAmount.value = response.user.amount // <-- or something like this
	} catch {
		console.log('Error fetching data')
	}
    return currentAmount.value
}

async function updateAmount(userID: number, newAmount: number) {
	try {
		const response = await fetch(`/api/users/${userID}/wallet/deposit`, {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
			},
			body: JSON.stringify({ amount: newAmount }),
		})
		deposited = true
	} catch (error) {
		console.error('Error updating data:', error)
	}
}
</script>

<template>
	<v-container>
        <v-row>
            <v-text v-if="err" >
                {{ err }}
            </v-text>
        </v-row>
		<v-row>
			<v-col>
				<v-text-field
					type="text"
					v-model="userID"
					placeholder="Set user ID..."
				></v-text-field>
			</v-col>
		</v-row>
		<v-row>
			<v-col>
				<v-text-field
					type="number"
					v-model="depositAmount"
					placeholder="Amount to deposit..."
				/>
				<v-btn @click="handleDepositInput">Deposit</v-btn>
			</v-col>
		</v-row>
		<!-- this should be reversed e.g. ="deposited"-->
		<v-row v-if="!deposited">
			<v-col>
				<p>
					Deposited: {{ renderDepositAmount }} into {{ userID }}'s
					account.
				</p>
			</v-col>
		</v-row>
	</v-container>
</template>
