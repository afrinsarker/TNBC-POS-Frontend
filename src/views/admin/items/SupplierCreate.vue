<template>
	<div>
		<div class="flex flex-nowrap justify-between mb-2 pt-4">
			<h1 class="display-h1">Add Supplier</h1>
			<div class="text-right">
				<button
					class="base-btn-outline mb-2" 
					@click="$router.go(-1)">
					Back
				</button>
			</div>
		</div>
		<div class="bg-white p-4 rounded-lg shadow-md">
			<div class="flex flex-col py-2">
				<label class="label" for="name">Name:</label>
				<input
					class="text-input" 
					type="text" 
					id="name" 
					v-model="name" 
					placeholder="John Doe"
					/>
			</div>
			<div class="flex flex-col py-2">
				<label class="label" for="email">Email:</label>
				<input
					class="text-input" 
					type="email" 
					id="email" 
					v-model="email" 
					placeholder="mail@example.com"
					/>
			</div>
			<div class="flex flex-col py-2">
				<label class="label" for="phone">Phone:</label>
				<input
					class="text-input" 
					type="phone" 
					id="phone" 
					v-model="phone" 
					placeholder="+33 6 7856 218"
					/>
			</div>
			<div class="flex flex-col py-2">
				<label class="label" for="company-name">Company name:</label>
				<input
					class="text-input" 
					type="text" 
					id="company-name" 
					v-model="companyName" 
					placeholder="ACME Inc."
					/>
			</div>
			<div class="my-2 text-right" v-show="$store.getters.userCan('create', 'Items')">
				<button class="base-btn-outline ml-2" @click="addSupplier">Save and create a new one</button>
				<button class="base-btn ml-2" @click="addSupplierAndRedirect">Save and exit</button>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import SupplierService from "@/services/items/SupplierService";
import ResponseData from "@/types/ResponseData";

export default defineComponent({
	name: 'SupplierCreate',
	data() {
		return {
			name: '',
			email: '',
			phone: '',
			companyName: ''
		}
	},
	methods: {
		async addSupplier(): Promise<void> {
			let data = {
				name: this.name,
				email: this.email,
				phone: this.phone,
				company: this.companyName
			}
			let token = this.$store.state.session.bearerToken
			await SupplierService.create(data, token)
				.then((response: ResponseData) => {
					this.$toast.open({
						message: `${this.name} successfully added to database!`,
						type: "success"
					})
				})
				.catch((e: Error) => {
					this.$toast.open({
						message: `There was an error adding that Supplier to the database.`,
						type: "error"
					})
					console.log(e)
				});
		},
		async addSupplierAndRedirect(): Promise<void> {
			let data = {
				name: this.name,
				email: this.email,
				phone: this.phone,
				company: this.companyName
			}
			let token = this.$store.state.session.bearerToken
			await SupplierService.create(data, token)
				.then((response: ResponseData) => {
					this.$toast.open({
						message: `${this.name} successfully added to database!`,
						type: "success"
					})
					this.$router.push({name: 'SupplierList'})
				})
				.catch((e: Error) => {
					this.$toast.open({
						message: `There was an error adding that Supplier to the database.`,
						type: "error"
					})
					console.log(e)
				});
		},
	}
});
</script>