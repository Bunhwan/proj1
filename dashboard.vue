<template>
  <v-container fluid class="pa-4" style="background-color: #f5f5f5;">
    <v-row justify="center" class="mb-5">
      <v-col cols="12" md="6">
        <v-card class="pa-5" color="#fff" elevation="3">
          <v-row align="center">
            <v-col>
              <h1 class="text-center mb-5">Dashboard</h1>
            </v-col>
          </v-row>

          <v-row class="mb-4">
            <v-col cols="12" class="text-center">
              <v-btn
                class="ma-2"
                color="primary"
                large
              >
                {{ status || 'Not Connected' }}
                <v-icon icon="mdi-checkbox-marked-circle" end></v-icon>
              </v-btn>
            </v-col>
          </v-row>

          <v-row class="mb-4">
            <v-col cols="12" md="6" class="text-center">
              <h4>Manual</h4>
              <v-progress-circular :model-value="value1" :rotate="360" :size="100" :width="15" color="teal">
                <template v-slot:default> {{ value1 }} % </template>
              </v-progress-circular>
              <v-col cols="auto">
                <v-btn size="small" color="success" @click="on1">ON</v-btn> 
                <v-btn @click="off1" size="small">OFF</v-btn>
              </v-col>

            </v-col>
            <v-col cols="12" md="6" class="text-center">
              <h4>Automatic</h4>
              <v-progress-circular :model-value="value2" :rotate="360" :size="100" :width="15" color="red">
                <template v-slot:default> {{ value2 }} % </template>
              </v-progress-circular>
              <v-col cols="auto">
                <v-btn size="small" color="success" @click="on2">ON</v-btn>
                <v-btn @click="off2" size="small">OFF</v-btn>
              </v-col>
            </v-col>
          </v-row>

          <v-row class="mb-3">
            <v-col cols="12">
            </v-col>
          </v-row>

          <v-row class="text-center">
            <v-col>
              
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import mqtt from "mqtt";
export default {
  data() {
    return {
      id: 1,
      msg: "",
      status: "",
      value1: 0,
      value2: 0,
      client: null,
    };
  },

  created() {
    this.client = mqtt.connect("ws://broker.emqx.io:8083/mqtt");
    this.client.on("connect", this.onMqttConnect);
    this.client.on("message", this.onMqttMessage);
    this.client.on("reconnect", this.handleOnReConnect);
  },

  beforeDestroy() {
    this.client && this.client.end();
  },

  methods: {
    on1() {
      this.value1 = 80;
      this.value2 = 0;
      this.client.publish("test/room", String(" Manual ON"));
      this.client.publish("test/room", String(" Automatic OFF"));
      
    },

    on2() {
      this.value2 = 80;
      this.value1 = 0;
      this.client.publish("test/room", String(" Automatic ON"));
      this.client.publish("test/room", String(" Manual OFF"));
    },

    off1() {
      this.value1 = 0;
      this.client.publish("test/room", String(" Manual OFF"));
    },

    off2() {
      this.value2 = 0;
      this.client.publish("test/room", String(" Automatic OFF"));
      
    },

    onMqttConnect() {
      this.status = "Mqtt connected";
      this.client.publish("op", "status");
      this.client.subscribe("status");
      this.client.subscribe("test/room");
    },

    onMqttMessage(topic, message) {
      if (topic === "status") {
        this.msg = message.toString();
      }
      if (topic === "room/sw01") {
        this.value = parseInt(message.toString(), 10);
      }
    },

    handleOnReConnect() {
      this.status = "Mqtt Reconnecting...";
      if (this.retryTimes > 5) {
        this.client.end();
        this.status = "Mqtt Disconnected";
      }
    },
  },
};
</script>

<style>
.text-center {
  text-align: center;
}

.mb-3, .mb-4, .mb-5 {
  margin-bottom: 1rem;
}

.v-card {
  border-radius: 8px;
}
</style>