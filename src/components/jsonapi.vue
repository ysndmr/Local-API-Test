<template>
  <div id="app">
    <div>
      <p v-if="!this.isShow">No Data, please first Save!</p>
      <table>
        <tr v-for="user in users" v-bind:key="user.id">
          <td>{{ user.name }}</td>
          <td>{{ user.surname }}</td>
          <td>{{ user.email }}</td>
        </tr>
      </table>

    </div>
    <button @click="fetchUsers"> Get users!</button>
    <div style="display: flex">
      <input type="text" v-model="putUser.name" name="name">
      <input type="text" v-model="putUser.surname" name="surname">
      <input type="text" v-model="putUser.email" name="mail">
    </div>
    <button @click="pushUsers"> Put users!</button>
    <div style="display: flex">
      <p>This filled with get method</p>
      <select name="getuser" id="users" @change="getUsers($event)">
        <option v-for="user in this.users"
                :key="user.key"
                :value="user.id">
          {{ user.name }} {{ user.surname }}
        </option>
      </select>
    </div>
    <button @click="removeUsers"> Delete users!</button>

  </div>


</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      users: [],
      putUser: {
        name: null,
        surname: null,
        email: null
      },
      selectedUser: null,
      isShow: false
    }
  },
  methods: {
    fetchUsers: function () {
      const baseURI = 'http://localhost:3000/users'
      this.$http.get(baseURI)
          .then((result) => {
            this.users = result.data;
            if (this.users.length > 0) {
              this.isShow = true;
            } else {
              alert('Get worked!')
            }

          })
    },
    pushUsers: function () {
      const baseURI = 'http://localhost:3000/users'
      this.$http.post(baseURI, this.putUser)
          .then((result) => {
            console.log(result);
            alert('Post worked!')
          });
      this.putUser = {
        name: null,
        surname: null,
        email: null
      }
    },
    getUsers(event) {
      this.selectedUser = event.target.value;
      alert('Get worked!')
    },
    removeUsers: function () {
      const baseURI = 'http://localhost:3000/users/' + this.selectedUser
      console.log(baseURI)
      this.$http.delete(baseURI)
          .then((result) => {
            console.log(result)
            alert('Remove worked!')
          })
    }
  }
}
</script>

<style>
#app {
  text-align: left;
  width: 700px;
  margin: 0 auto;
  color: #2c3e50;
  margin-top: 60px;
}

input, select {
  font-size: 0.875rem;
  font-weight: 300;
  color: #017a72;
  line-height: 2.375rem;
  min-height: 3rem;
  position: relative;
  border: 3px solid #E8E8E8;
  border-radius: 0;
  background: #fff;
  padding: 0 0.8125rem;
  transition: border 0.1s ease;
  box-sizing: border-box;
  box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.3);
  width: 33.333333%;
  margin: 5px 10px;
}

button {
  line-height: 50px;
  height: 50px;
  text-align: center;
  width: 250px;
  cursor: pointer;
  color: #FFF;
  border: 0;
  background: rgba(0, 0, 0, 0.6);
  transition: all 0.5s;
  position: relative;
  letter-spacing: 2px;
  font-size: 1rem;
  margin: 15px 0;
}

table {
  display: inline-table;
  width: 100%;
  width: -moz-max-content;
  max-width: 100%;
  overflow: auto;
}

table thead {
  background: rgba(0, 0, 0, 0.2);
}

table tr {
  background: rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

table td,
table th {
  text-align: left;
  padding: 10px;
  background: rgba(0, 0, 0, 0.6);
  color: #fff;
}

table th {
  font-weight: 600;
}

table tr:hover {
  background: rgba(0, 0, 0, 0.4);
}
</style>