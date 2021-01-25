<template lang="">
  <div>
    <b-form @submit.prevent="onSubmit">
      <b-form-group id="input-group-2" label="Username:" label-for="input-1" description="This is required field.">
        <b-form-input id="username" name="username" v-model="form.username" placeholder="Enter username" required></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-1" label="Email address:" label-for="input-2"
        description="We'll never share your email with anyone else.">
        <b-form-input id="email" name="email" v-model="form.email" type="email" placeholder="Enter email" required></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="First Name:" label-for="input-3" description="This is required field.">
        <b-form-input id="firstname" name="firstname" v-model="form.firstname" placeholder="Enter first name" required></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="Last Name:" label-for="input-4" description="This is required field.">
        <b-form-input id="lastname" name="lastname" v-model="form.lastname" placeholder="Enter last name" required></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
    <br>
    <div class="card">
      <!---->
      <div class="card-header">
        <h4 class="card-title">Users</h4>
      </div>
      <!---->
      <div class="table-responsive" v-if="users.length">
        <table class="table table-striped">
          <thead>
            <th>Username</th>
            <th>Email</th>
            <th>Firstname</th>
            <th>Lastname</th>
            <th>Edit/Delete</th>
          </thead>
          <tbody>
            <tr v-for="(user, index) in users" :key="user.id">
              <td>{{ user.username }}</td>
              <td>{{ user.email }}</td>
              <td>{{ user.firstname }}</td>
              <td>{{ user.lastname }}</td>
              <td>
                <b-button size="sm" variant="warning" @click="editUser(index)"><span class="ti-pencil"></span></b-button> / 
                <b-button size="sm" variant="danger" @click="remove(index)"><span class="ti-trash"></span></b-button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <h3 class="text-center" v-else>You need to add a user first</h3>
      <!---->
    </div>

    <b-modal id="editModal" title="Edit" hide-footer v-for="(data, index) in editUserData" :key="data.id">
      <b-form @submit.prevent="updateUser(data.id, index)">
        <b-form-group id="input-group-2" label="Username:" label-for="input-1" description="This is required field.">
          <b-form-input id="editusername" name="editusername" v-model="data.username" placeholder="Enter username" required></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-1" label="Email address:" label-for="input-2"
          description="We'll never share your email with anyone else.">
          <b-form-input id="editemail" name="editemail" v-model="data.email" type="email" placeholder="Enter email" required></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="First Name:" label-for="input-3" description="This is required field.">
          <b-form-input id="editfirstname" name="editfirstname" v-model="data.firstname" placeholder="Enter first name" required></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="Last Name:" label-for="input-4" description="This is required field.">
          <b-form-input id="editlastname" name="editlastname" v-model="data.lastname" placeholder="Enter last name" required></b-form-input>
        </b-form-group>

        <b-button type="submit" variant="primary btn-block">CONFIRM</b-button>
      </b-form>
    </b-modal>
  </div>
</template>
<script>
  import NotificationTemplate from './Notifications/NotificationTemplate';

  let nextUserId = 1;

  export default {
    data() {
      return {
        form: [{
          id: '',
          username: '',
          email: '',
          firstname: '',
          lastname: '',
        }],
        users: [
          // { id: nextUserId++, username: "marc", email: "marc@yahoo.com", firstname: "Marc", lastname: "Asoy" },
        ],
        editUserData: [{
          id: '',
          username: '',
          email: '',
          firstname: '',
          lastname: '',
        }],
      }
    },
    methods: {
      onSubmit() {
        const trimmedUsername = this.form.username.trim()
        const trimmedEmail = this.form.email.trim()
        const trimmedFirstname = this.form.firstname.trim()
        const trimmedLastname = this.form.lastname.trim()

        if(trimmedUsername) {
          
          this.users.push({
            id: nextUserId++,
            username: trimmedUsername,
            email: trimmedEmail,
            firstname: trimmedFirstname,
            lastname: trimmedLastname
          });

          this.form.username = '';
          this.form.email = '';
          this.form.firstname = '';
          this.form.lastname = '';
        }
        
        this.$notify({
          component: NotificationTemplate,
          icon: "ti-gift",
          verticalAlign: "top",
          horizontalAlign: "center",
          type: "success",
          message: "User Successfully added!",
        });
      },

      remove(userIndex) {
        this.users.splice(userIndex, 1);
        this.$notify({
          component: NotificationTemplate,
          icon: "ti-gift",
          verticalAlign: "top",
          horizontalAlign: "center",
          type: "danger",
          message: "User Successfully remove!",
        });
      },

      editUser(userIndex) {
        this.$bvModal.show("editModal")
        this.editUserData = this.users.slice(userIndex);
        // this.form.push(copiedUser);
      },

      updateUser(userId, userIndex) {
        for(var i in this.users) {
          if(this.users[i].id == userId) {
            this.users[i] ==  this.editUserData.slice(userIndex)
          }
        }
        
        this.$notify({
          component: NotificationTemplate,
          icon: "ti-gift",
          verticalAlign: "top",
          horizontalAlign: "center",
          type: "warning",
          message: "User Successfully updated!",
        });

        this.$bvModal.hide("editModal")
      }
    }
  }

</script>
<style lang="">

</style>
