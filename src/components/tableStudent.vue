<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="employeeItem"
      sort-by="calories"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>จัดการข้อมูลนักศึกษา</v-toolbar-title>

          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>

          <v-btn
            color="primary"
            dark
            class="mb-2"
            @click="openDialog('add', defaultItem)"
          >
            เพิ่มข้อมูลนักศึกษา
          </v-btn>
        </v-toolbar>
      </template>

   
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="openDialog('edit', item)">
          mdi-pencil
        </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="text-h5">{{ formTitle }}</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-text-field v-model="firstname" label="ชื่อ"></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field v-model="lastname" label="นามสกุล"></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field v-model="email" label="อีเมลล์"></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field v-model="studentnumber" label="รหัสนักศึกษา"></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="closeDelete()">
            ยกเลิก
          </v-btn>
          <v-btn color="blue darken-1" text @click="save(formTitle)">
            บันทึกข้อมูล
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogDelete" max-width="500px">
      <v-card>
        <v-card-title class="text-h5"
          >Are you sure you want to delete this item?</v-card-title
        >
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="deleteItemConfirm"
            >OK</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data: () => ({
    firstname: "",
    lastname: "",
    email: "",
    studentnumber: '',
    idStudent: "",

    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "ไอดี",
        align: "start",
        sortable: false,
        value: "id",
      },
      { text: "ชื่อ", value: "firstName" },
      { text: "นามสกุล", value: "lastName" },
      { text: "อีเมลล์", value: "email" },
      { text: "รหัสนักศึกษา", value: "studentNumber" },
      { text: "Actions", value: "actions", sortable: false },
    ],

    employeeItem: [],
    editedIndex: -1,
    editedItem: {
      name: "",
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0,
    },
    defaultItem: {
      name: "",
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0,
    },
    formTitle: "",

    idFordelete: "",
  }),

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    async initialize() {
      this.employeeItem = [];
      try {
        var data = await this.axios.get("http://localhost:9000/students");
        console.log("data student ====>", data);
        this.employeeItem = data.data;
      } catch (error) {}
    },

    openDialog(Action, item) {
      // console.log(Action,item);
      this.formTitle = "";
      if (Action === "add") {
        this.dialog = true;
        this.formTitle = "เพิ่มข้อมูล";
        this.editItem = item;
      } else {
        console.log(Action, item);
        this.dialog = true;
        this.formTitle = "แก้ไขข้อมูล";
        // this.editedIndex = this.desserts.indexOf(item);
        // this.editedItem = item;
        this.firstname = item.firstName;
        this.lastname = item.lastName;
        this.email = item.email;
        this.studentnumber = item.studentNumber
;
        this.idStudent = item.id;
      }
    },
    editItem(item) {
      //   console.log("item select", item);
      //   console.log("item item", this.desserts.indexOf(item));

      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.idFordelete = item.id;
      this.dialogDelete = true;
    },

    async deleteItemConfirm() {
      try {
        var response = await this.axios.delete(
          "http://localhost:9000/students/" + this.idFordelete
        );
        this.initialize();
      } catch (error) {
        console.log(error.massage);
      }
      this.closeDelete();
    },

    closeDelete() {
      this.dialogDelete = false;
      this.editedItem = [];
      this.editedIndex = -1;
    },
    close() {
      this.dialog = false;
      (this.firstname = ""),
        (this.lastname = ""),
        (this.email = ""),
        (this.studentnumber = "");
    },
    async save(action) {
      var data = {
        firstName: this.firstname,
        lastName: this.lastname,
        email: this.email,
        studentNumber:this.studentnumber
        
      };
      if (action === "เพิ่มข้อมูล") {
        // this.desserts.push(this.editedItem);
        // this.dialog = false;
        // this.close();
        console.log("data after send ====>", data);
        try {
          var dataResponse = await this.axios.post(
            "http://localhost:9000/students",
            data
          );
          console.log("dataReponse ===>", dataResponse);
          this.close();
          this.initialize();
        } catch (error) {
          console.log(error.massage);
        }
      } else {
        try {
          var dataReponseEdit = await this.axios.put(
            "http://localhost:9000/students/" + this.idStudent,
            data
          );
          console.log("dataReponseEdit ===>", dataReponseEdit);
          this.close();
          this.initialize();
        } catch (error) {
          console.log(error.massage);
        }
        Object.assign(this.desserts[this.editedIndex], this.editItem);
      }
      this.close();
    },
  },
};
</script>

<style></style>
