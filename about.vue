<template>
    <v-container>
      <!-- Team Member Profile Section -->
      <v-row>
        <v-col cols="12" md="6" class="mx-auto">
          <v-card>
            <v-img
            style="margin-bottom: 20px;"
            src="c:\img\tawan2.png"
            height="300px"
            class="white--text"
            >
              <v-card-title class="display-2">{{ teamMember.name }}</v-card-title>
            </v-img>
            <v-card-text>
              <h2>{{ teamMember.position }}</h2>
              <p>
                {{ teamMember.bio }}
              </p>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  export default {
    data: () => ({
      teamMember: {
        name: 'ตะวัน บุญหว่าน',
        position: 'CEO',
        bio: 'ตะวัน บุญหว่าน เป็นผู้นำที่มีวิสัยทัศน์ของบริษัทของเรา ด้วยประสบการณ์กว่า 15 ปีในอุตสาหกรรมเทคโนโลยี ความหลงใหลในนวัตกรรมของเขาขับเคลื่อนทิศทางกลยุทธ์ของเราและสร้างแรงบันดาลใจให้ทีมของเราไปสู่ความสำเร็จใหม่ๆ',
      }
    })
  }
  </script>
  
  <style scoped>
  .v-container {
    padding-top: 20px;
  }
  .v-img {
    background-size: cover;
    background-position: center;
  }
  .v-card {
    margin-top: 20px;
  }
  </style>
  