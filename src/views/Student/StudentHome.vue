/* eslint-disable */
<template>
  <v-content>
    <v-navigation-drawer app clipped>
      <v-list>
        <v-list-item link>
          <v-list-item-content>
            <v-list-item-title class="title">
              {{ $store.getters.getUserData.nameTH }}
            </v-list-item-title>
            <v-list-item-subtitle
              >role :
              {{ $store.getters.getUserData.role }}</v-list-item-subtitle
            >
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-divider></v-divider>

      <v-list nav dense>
        <v-list-item link @click="menuSelect(1)">
          <v-list-item-icon>
            <v-icon color="blue">mdi-folder</v-icon>
          </v-list-item-icon>
          <router-link to="/student"></router-link>
          <v-list-item-title>หน้าแรก</v-list-item-title>
        </v-list-item>

        <v-list-item link @click="menuSelect(2)">
          <v-list-item-icon>
            <v-icon>mdi-account-multiple</v-icon>
          </v-list-item-icon>
          <v-list-item-title>ข้อมูลส่วนตัว</v-list-item-title>
        </v-list-item>

        <v-list-item link @click="menuSelect(3)">
          <v-list-item-icon>
            <v-icon>mdi-star</v-icon>
          </v-list-item-icon>
          <v-list-item-title>คะแนนรายวิชา</v-list-item-title>
        </v-list-item>

        <v-list-item link @click="menuSelect(4)">
          <v-list-item-icon>
            <v-icon>mdi-star</v-icon>
          </v-list-item-icon>
          <v-list-item-title>เกรดเฉลี่ย</v-list-item-title>
        </v-list-item>

        <v-list-item link @click="menuSelect(5)">
          <v-list-item-icon>
            <v-icon>mdi-star</v-icon>
          </v-list-item-icon>
          <v-list-item-title>ข้อมูลอาจารย์</v-list-item-title>
        </v-list-item>

        <v-list-item
          link
          @click="menuSelect(6)"
          v-if="$store.getters.getPollState == 'true'"
        >
          <v-list-item-icon>
            <v-icon>mdi-star</v-icon>
          </v-list-item-icon>
          <v-list-item-title>โหวดอาจารย์ดีเด่น</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main class="pa-0 content">
      <v-container>
        <!-- <section class="mb-10">
          <v-sheet height="600" color="green" class="pa-10">
            <h1 class="text-center">พื้นที่สำหรับตารางเรียน</h1>
          </v-sheet>
        </section> -->
        <v-row>
          <v-col lg="4" cols="12" v-for="i in postList" :key="i">
            <v-card :color="(i.priority=='1') ? 'red' : '#26c6da'" dark max-width="420">
              <v-card-title>
                <v-icon large left> mdi-bullhorn </v-icon>
                <span class="title font-weight-light">{{ i.title }}</span>
              </v-card-title>

              <v-card-text class="headline font-weight-bold">
                {{ i.Message }}
              </v-card-text>

              <v-card-actions>
                <v-list-item class="grow">
                  <v-list-item-avatar>
                    <v-img
                      class="elevation-6"
                      alt=""
                      src="https://cdn.discordapp.com/emojis/803877390786494465.png?v=1"
                    ></v-img>
                  </v-list-item-avatar>

                  <v-list-item-content>
                    <v-list-item-title>{{
                      i.FnameTH + " " + i.LnameTH
                    }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-content>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      postList: "",
    };
  },
  methods: {
    menuSelect(select) {
      if (select == 2) this.$router.push({ name: "StudentInformation" });
      else if (select == 3) this.$router.push({ name: "Score" });
      else if (select == 4) this.$router.push({ name: "GradeView" });
      else if (select == 5) this.$router.push({ name: "TeacherInfo" });
      else if (select == 6) this.$router.push({ name: "VoteTeacher" });
    },
    fetchPost() {
      axios
        .get("/user/student/get/advisor", {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("userKey")}`,
          },
          params: {
            ClassroomID: this.$store.getters.getUserData.ClassroomID,
          },
        })
        .then((res) => {
          axios
            .get("/user/post/getPost", {
              headers: {
                Authorization: `Bearer ${localStorage.getItem("userKey")}`,
              },
              params: {
                PostById: res.data.data[0].citizenID,
              },
            })
            .then((res) => {
              this.postList = res.data.data;
            });
        });
    },
  },
  created() {
    this.fetchPost();
  },
};
</script>

<style scoped>
.content {
  max-width: 1300px;
  margin: 50px auto;
}
</style>
