<template>
  <b-row>
    <b-col cols="12">
      <h2>
        Room List
        <b-link href="#/add-room">(Add Room)</b-link>
      </h2>
      <b-table striped hover outlined :items="rooms" :fields="fields">
        <template slot="actions"  scope="row">
          <b-btn size="sm" @click.stop="join(row.item._id)">Join 1 </b-btn>
        </template>
      </b-table>
      <ul v-if="errors && errors.length">
        <li v-for="error of errors">
          {{error.message}}
        </li>
      </ul>
    </b-col>
  </b-row>
</template>

<script>

import axios from 'axios'

export default {
  name: 'BookList',
  data () {
    return {
      fields: {
        room_name: { label: 'Room Name', sortable: true, 'class': 'text-center' },
        created_date: { label: 'Created Date', sortable: true },
        _id: {label: 'ID', sortable:true},
        actions: { label: 'Action', 'class': 'text-center' }
      },
      rooms: [],
      errors: []
    }
  },
  created () {
    axios.get(`http://localhost:3000/api/room`)
    .then(response => {
      this.rooms = response.data
      console.log(this.rooms);
    })
    .catch(e => {
      this.errors.push(e)
    })
  },
  methods: {
    join (id) {
      console.log("BUTTON CLICKED.. ID = ", id );
      this.$router.push({
        name: 'JoinRoom',
        params: { id: id }
      })
    }
  }
}
</script>