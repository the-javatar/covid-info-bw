<template>
<v-container>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="600px">
      <template v-slot:activator="{ on }">
          <br><br>
        <v-btn color="deep-purple white--text" dark v-on="on">Send Message</v-btn>
       
      </template>
      <v-card>
        <v-card-title>
          <span class="headline">Leave your message.</span>
        </v-card-title>
        
        <v-card-text>
          <v-container>
            <p>Share your thoughts, positive comments or something funny with your community.</p>
           <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field id="name"
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="Name"
      required
    ></v-text-field>

    <v-text-field id="place"
      v-model="place"
      :rules="nameRules"
      label="Place"
      required
    ></v-text-field>

    <v-textarea id="message"
      v-model="message"
      label="Message"
      required
    ></v-textarea>

    <v-checkbox
      v-model="checkbox"
      :rules="[v => !!v || 'You must agree to continue!']"
      label="My message does not include any hate speech or vulgar language and serves only as words of encouragement."
      required
    ></v-checkbox>

    <v-btn
      :disabled="!valid"
      color="deep-purple white--text"
      class="mr-4"
      @click="submit"
      :loading="loading"
    >
      Send
    </v-btn>

  </v-form>
          <small>*indicates required field</small>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red white--text" @click="dialog = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>

 </v-container>
</template>

<script>
import { db } from '../assets/utilities/db'

  export default {
    data: () => ({
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'This field is required',
        v => (v && v.length <= 20) || 'Field must be less than 20 characters',
      ],
      place: '',
      message: '',
      dialog: false,
      checkbox: false,
      loading: false
    }),
    methods: {
      submit () {
          let date = new Date();
          if(this.$refs.form.validate()){
              this.loading = true
                const message = { 
                name: this.name,
                place: this.place,
                message: this.message,
                createdAt: date,
                approved: 0
                }

            db.collection('messages').add(message).then(() => {
                    this.loading = false
                    this.dialog = false
                console.log('added to db')
            })

            this.name = "";
            this.place = "";
            this.message = "";
          }
        
      },
    }
  }
</script>