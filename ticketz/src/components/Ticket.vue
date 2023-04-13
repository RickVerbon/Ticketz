<template>
    <div v-if="ticket" class="col-md-9">
            <div class="card">
                <div class="card-header ticket-header">
                    {{ ticket.title }}
                </div>  
                <div class="card-body">
                <div class="date">Date created: {{new Date(ticket.created_at).toLocaleString('en-GB') }}</div>
                <div class="description">
                    {{ticket.request}}
                </div>
                <div class="comments">
                    <form @submit.prevent>
                    <div class="form-group">
                        <textarea v-model="newCommentText" class="form-control" rows="3" placeholder="Add a comment..."></textarea>
                    </div>
                    <button type="submit" @click="addComment" class="btn btn-primary">Add comment</button>
                    </form>
                    <div v-for="comment in comments" class="comment">
                    <p>{{ comment.text }}</p>
                    <small>By User {{comment.author}} on {{new Date(comment.created_at).toLocaleString('en-GB') }}</small>
                    </div>
                </div>
                </div>
            </div>
            </div>
            <div v-else>
                <p>Loading ticket</p>
            </div>

    </template>

<style scoped>
.card {
  background-color: #CFEBDF;
  border-radius: 10px;
  border: none;
  
  margin-top: 20px;
}

.card-header {
  border-radius: 10px 10px 0 0;
  background-color: #5F634F;
  color: #E2FADB;
}

.date {
  margin-bottom: 10px;
}

.description {
  margin-bottom: 20px;
}

.comments {
  border-top: 1px solid #5F634F;
  padding-top: 20px;
}

.comment {
  margin-top: 20px;
  border-left: 2px solid #9BC4CB;
  padding-left: 10px;
}

.comment p {
  margin-bottom: 5px;
}

.comment small {
  color: #5F634F;
}

.ticket-header {
  background-color: #5F634F;
  border-radius: 10px 10px 0 0;
  color: white;
  padding: 10px;
}

button[type="submit"] {
  background-color: #5F634F;
  color: #E2FADB;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
}

button[type="submit"]:hover {
  background-color: #9BC4CB;
  cursor: pointer;
}

.input-field {
  margin-bottom: 10px;
}

.input-field label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

.input-field input {
  width: 100%;
  padding: 5px;
  border: none;
  border-radius: 5px;
}
</style>

<script>
import axios from 'axios'
export default {
	data() {
		return {
			TICKET_URL: "http://127.0.0.1:8000/api/tickets/" + this.$route.params.id,
            ticket: null,
            newCommentText: "",
            comments: []
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