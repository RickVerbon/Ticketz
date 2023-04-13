<template>
    <div class="container">
      <div class="row">
        <PanelComponent/>
        <TicketComponent/>
      </div>
    </div>
  </template>


<style scoped>
</style>

<script>
import axios from 'axios'
import router from '../router/index.js'
import TicketComponent from '../components/Ticket.vue'
import PanelComponent from '../components/Panel.vue'

export default {
    components: {
        TicketComponent,
        PanelComponent
    },
	data() {
		return {
			TICKET_URL: "http://127.0.0.1:8000/api/tickets/" + this.$route.params.id,
            COMMENT_URL: this.TICKET_URL + "/comments",
            ticket: null,
            comments: [],
            newCommentText: ""
		}
	},
    computed: {
    	isAuthenticated() {
      		return localStorage.getItem('ticketz_token') !== null;
    	}
    },
	methods: {
        async getTicket() {
            const token = localStorage.getItem('ticketz_token')
            try {
                const response = await axios.get(this.TICKET_URL, {
                    headers: {
                        Authorization: `Token ${token}`
                    }
            });
            this.ticket = response.data
            this.comments = response.data.comments
            } catch(error) {
                console.error(error)
            }
        },
        async addComment() {
            const token = localStorage.getItem('ticketz_token')
            const config = {
                headers: {
                    Authorization: `Token ${token}`,
                    'Content-Type': 'application/json',
                },
            }
            try {
                const data = {
                    ticket: this.ticket.id,
                    text: this.newCommentText
                }
                const response = await axios.post(this.COMMENT_URL, data, config); 
                this.comments.push(response.data)
                this.newCommentText = ""
            } catch(error) {
                console.error(error)
            }
	    },
    },
    created() {
        if(!this.isAuthenticated) {
        	router.push({name: "login"})
        return
      	}
        this.TICKET_URL = "http://127.0.0.1:8000/api/tickets/" + this.$route.params.id;
        this.COMMENT_URL = this.TICKET_URL + "/comments";
        this.getTicket();
    },  
}
</script>