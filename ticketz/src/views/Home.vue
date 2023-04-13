<template>
 	<div class="container ticket-list">
		<h1>List of Tickets</h1>
		<table class="table">
			<thead>
				<tr>
					<th>ID</th>
					<th>Title</th>
					<th>Date/Time Created</th>
					<th>Operator</th>
				</tr>
			</thead>
			<tbody v-for="ticket in tickets" :key="ticket.id">
				<tr>
					<td>{{ticket.id}}</td>
					<td>
						<router-link :to="'ticket/' + ticket.id">
							{{ticket.request}}
						</router-link> 
					</td>
					<td>{{new Date(ticket.created_at).toLocaleString('en-GB') }}</td>
					<td>Operator</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
import axios from 'axios'
import router from '../router/index.js'

export default {
	data() {
		return {
			GET_TICKETS_URL: "http://127.0.0.1:8000/api/tickets",
			tickets: null
		}
	},
	computed: {
    	isAuthenticated() {
      		return localStorage.getItem('ticketz_token') !== null;
    	}
    },
	methods: {
		async getAllTickets() {
			const token = localStorage.getItem('ticketz_token')
			try {
				const response = await axios.get(this.GET_TICKETS_URL, {
					headers: {
						Authorization: `Token ${token}`
					}
				});
				this.tickets = response.data
			} catch (error) {
				console.error(error)
			}	
		},
	},
	created() {
		if(!this.isAuthenticated) {
        	router.push({name: "login"})
        return
      	}
		this.getAllTickets()
	}
}
</script>