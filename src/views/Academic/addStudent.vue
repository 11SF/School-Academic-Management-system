<template>
  <v-content>
    <v-container class="mt-10">
      <v-form  ref="form" lazy-validation>
        <h1>ข้อมูลส่วนตัว</h1>
        <v-row class="mt-10">
          <v-col cols="12" lg="6">
            <v-card class="pa-5" rounded="xl">
              <v-row dense>
                <v-col cols="12" lg="5" align-self="center" class="text-center">
                  <v-avatar
                    size="180"
                    class="d-flex mx-auto outlined"
                    color="#81c784"
                  >
                  </v-avatar>
                  <v-chip class="ma-2" color="#81c784" dark> Student </v-chip>
                </v-col>
                <v-col cols="12" lg="7" class="pa-5">
                  <v-text-field
                    v-model="profile.FnameTH"
                    label="ชื่อ"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="profile.LnameTH"
                    label="สกุล"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="profile.FnameEN"
                    label="Firstname"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="profile.LnameEN"
                    label="Lastname"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="profile.CitizenID"
                    label="เลขบัตรประชาชน"
                    type="number"
                    required
                  ></v-text-field>
                  <div class="d-flex">
                    <v-text-field
                      v-model="profile.Nickname"
                      label="ชื่อเล่น"
                      required
                    ></v-text-field>
                    <v-text-field label="เพศ"></v-text-field>
                  </div>
                </v-col>
              </v-row>
            </v-card>
            <v-row>
              <v-col cols="12">
                <v-card class="pa-10" rounded="xl">
                  <h4>ข้อมูลติดต่อ</h4>
                  <div class="px-5 pt-5 d-flex justify-content-around">
                    <p class="text-center pr-5">
                      <v-text-field
                        v-model="profile.phone"
                        label="เบอร์โทรศัพท์"
                        required
                      ></v-text-field>
                    </p>
                    <p class="text-center">
                      <v-text-field
                        v-model="profile.email"
                        label="email"
                        required
                      ></v-text-field>
                    </p>
                  </div>
                </v-card>
              </v-col>
            </v-row>
          </v-col>
          <v-col cols="12" lg="6">
            <v-card class="pa-10" rounded="xl" height="502">
              <v-text-field
                v-model="profile.Bdate"
                label="วันเดือนปีเกิด"
                required
              ></v-text-field>
              <v-text-field
                v-model="profile.Location"
                label="ที่อยู่"
                required
              ></v-text-field>
              <v-text-field
                v-model="profile.zip"
                label="รหัสไปรษณีย์"
                required
              ></v-text-field>
              <br />
              <!-- <v-divider></v-divider> -->
              <br />
              <v-text-field
                v-model="ability"
                label="ความสามารถพิเศษ"
                required
              ></v-text-field>
            </v-card>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-card class="pa-10" rounded="xl">
              <h4>ผู้ปกครอง</h4>
              <v-row d-flex justify="center">
                <v-col cols="12" lg="6" align-self="center">
                  <div class="px-10">
                    <v-text-field
                      v-model="dependent.Fname"
                      label="ชื่อ"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="dependent.Lname"
                      label="สกุล"
                      required
                    ></v-text-field>
                    <v-text-field
                      v-model="dependent.Nickname"
                      label="ชื่อเล่น"
                      required
                    ></v-text-field>
                  </div>
                </v-col>
                <v-col cols="12" lg="6">
                  <v-text-field
                    v-model="dependent.phone"
                    label="โทรศัพท์"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="dependent.relation"
                    label="ความสัมพันธ์"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
        </v-row>
        <div class="text-end">
          <v-btn
            class="ma-2"
            :loading="loading"
            :disabled="loading"
            color="success"
            @click="(snackbar = true), saveData()"
          >
            บันทึกข้อมูล
          </v-btn>
        </div>
      </v-form>

      <v-snackbar v-model="snackbar" :timeout="timeout">
        บันทึกสำเร็จ

        <template v-slot:action="{ attrs }">
          <v-btn color="blue" text v-bind="attrs" @click="snackbar = false">
            Close
          </v-btn>
        </template>
      </v-snackbar>
    </v-container>
  </v-content>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      profile: {
        FnameTH: "",
        LnameTH: "",
        FnameEN: "",
        LnameEN: "",
        CitizenID: "",
        Nickname: "",
        phone: "",
        email: "",
        Bdate: "",
        Location: "",
        zip: "",
        ClassroomID: this.$store.getters.getRoomSelect.ClassroomID,
      },
      dependent: {
        Fname: "",
        Lname: "",
        Nickname: "",
        phone: "",
        relation: "",
      },
      ability: "",
    };
  },
  methods: {
    saveData() {
      axios
        .get("/user/student/add", {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("userKey")}`,
          },
          params: {
            profile: this.profile,
            dependent: this.dependent,
            ability: this.ability,
          },
        })
        .then((res) => {
          if (res.data.status) {
            this.$router.push("Class");
          }
        });
    },
  },
};
</script>

<style>
</style>