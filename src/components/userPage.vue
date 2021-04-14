<template>
<div>
 <v-data-table
    :headers="headers"
    :items="users"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>User List</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              New User
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedUser.firstName"
                      label="First Name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedUser.lastName"
                      label="Last Name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedUser.age"
                      label="Age"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedUser.email"
                      label="Email"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedUser.companyName"
                      label="Company Name"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">Are you sure you want to delete this user?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteUserConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editUser(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteUser(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
    <v-snackbar
      v-model="snackbar"
      :timeout="timeout"
      :right="right"
      :color="color"
    >
    {{snackbarText}}
    <v-btn text @click="snackbar = false">Close</v-btn>
    </v-snackbar>
</div>
</template>


<script>

  export default {
    data: () => ({
      snackbar: false,
      timeout:3000,
      right: true,
      color: "success",
      snackbarText:"",
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'First Name',
          align: 'start',
          sortable: false,
          value: 'firstName',
        },
        { text: 'Last Name', value: 'lastName' },
        { text: 'Age', value: 'age' },
        { text: 'Email', value: 'email' },
        { text: 'Company Name', value: 'companyName' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      users: [],
      editedIndex: -1,
      editedUser: {
        firstName: '',
        lastName: '',
        age: 0,
        email: '',
        companyName:''
      },
      defaultUser: {
        firstName: '',
        lastName: '',
        age: 0,
        email: '',
        companyName:''
      },
    }),
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New User' : 'Edit User'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.users = [
          {
           firstName: 'Cengiz',
           lastName: 'Cebeci',
           age: 23,
           email: 'theJengo@gmail.com',
           companyName:'Innovile'
          },
          {
           firstName: 'Canberk',
           lastName: 'Ateş',
           age: 23,
           email: 'canberkAtes@gmail.com',
           companyName:'Innovile'
          },
          {
           firstName: 'Barış',
           lastName: 'Elvanoğlu',
           age: 23,
           email: 'barisElvanoglu@gmail.com',
           companyName:'Innovile'
          },
          {
           firstName: 'Alper',
           lastName: 'Şentürk',
           age: 23,
           email: 'alperSenturk@gmail.com',
           companyName:'Innovile'
          },
          {
           firstName: 'Ömer',
           lastName: 'Savaş',
           age: 23,
           email: 'omersavas@gmail.com',
           companyName:'Innovile'
          },
          {
           firstName: 'Hikmet',
           lastName: 'Dönmez',
           age: 24,
           email: 'hikmettdonmez@gmail.com',
           companyName:'Innovile'
          } 
        ]
      },

      editUser (item) {
        this.editedIndex = this.users.indexOf(item)
        this.editedUser = Object.assign({}, item)
        this.dialog = true
      },

      deleteUser (item) {
        this.editedIndex = this.users.indexOf(item)
        this.editedUser = Object.assign({}, item)
        this.dialogDelete = true
      },
    
      deleteUserConfirm () {
        this.users.splice(this.editedIndex, 1)
        this.closeDelete()
        this.snackbar = true,
        this.snackbarText = "User deleted successfully"
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedUser = Object.assign({}, this.defaultUser)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedUser = Object.assign({}, this.defaultUser)
          this.editedIndex = -1
        })
      },

      save () {
        
      },
    },
  }
</script>

<style>
@import url("https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css");
</style>