<template>
  <v-app>
    <v-main>
      <v-container fluid class="pa-0 h-100">
        <v-row class="flex-column h-100" no-gutters>
          <v-col class="flex-grow-0"
            ><v-toolbar flat>
              <v-icon
                large
                class="mr-5"
                color="black"
                @click="callNativeApp('back form javascript')"
                >mdi-chevron-left</v-icon
              >
              <v-img
                max-width="48"
                src="https://picsum.photos/id/1005/48/48"
                class="rounded-circle mr-4"
                @click="callNativeApp('uid : test0001')"
              ></v-img>
              <v-toolbar-title>Angus</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-icon
                color="black"
                @click="callNativeApp('more form javascript')"
                >mdi-dots-vertical</v-icon
              >
            </v-toolbar></v-col
          >
          <v-col style="overflow-y: scroll;"
            ><v-card flat class="d-flex flex-column pt-3">
              <v-btn text color="#7E7E7E" class="mb-5">
                向對方打聲招呼~
              </v-btn>
              <v-btn
                depressed
                rounded
                color="#F0F0F0"
                class="align-self-center mb-5"
              >
                2020/11/28 星期六
              </v-btn>
              <ul class="px-4" v-for="(item, index) in msgs" :key="index">
                <li
                  class="d-flex align-center mb-5"
                  :class="{ reverse: !item.type }"
                >
                  <div
                    class="text-box-text"
                    :class="item.type ? 'custom' : 'main'"
                  >
                    {{ item.msg }}
                  </div>
                  <div class="text-box-time">{{ item.time }}</div>
                </li>
              </ul>
            </v-card></v-col
          >
          <v-col class="flex-grow-0">
            <v-row style="background-color:#E0E0E0;">
              <v-col cols="12" class="d-flex justify-space-between px-6">
                <v-icon>
                  mdi-plus-circle
                </v-icon>
                <v-icon>
                  mdi-image
                </v-icon>
                <input
                  v-model="msg"
                  type="text"
                  class="textbox"
                  style="width: 200px;
                background-color: #bdbdbd;
                height: 32px;
                border-radius: 16px;
                padding: 12px;"
                />
                <v-icon @click="sendMsg">
                  mdi-send
                </v-icon>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </v-main></v-app
  >
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    msgs: [
      {
        name: 'Victor',
        type: true,
        time: '8:20 PM',
        msg: '晚安，今天練舞還好嗎？',
      },
      {
        name: 'Angus',
        type: false,
        time: '9:20 PM',
        msg: '今天練一整天，現在好累',
      },
    ],
    msg: '',
    websock: null,
  }),
  methods: {
    sendMsg() {
      const vm = this;
      let msg = {
        name: 'Victor',
        type: false,
        time: '10:40 PM',
        msg: vm.msg,
      };
      vm.msgs.push(msg);
      this.msg = '';
    },
    callNativeApp(data) {
      try {
        webkit.messageHandlers.callbackHandler.postMessage(data);
      } catch (err) {
        console.log('The native context doesnt exist yet!');
      }
    },
    click() {
      console.log('click');
    },
    initWebsocket() {
      const wsUrl = 'ws://t3-dating-api.azurewebsites.net/chat';
      // const wsUrl = 'ws://localhost:3000/chat';
      this.websock = new WebSocket(wsUrl);

      this.websock.onmessage = this.websocketonmessage;
      this.websock.onopen = this.websocketonopen;
      this.websock.onerror = this.websocketonerror;
      this.websock.onclose = this.websocketclose;
    },
    websocketonopen() {
      console.log('open connection');
      let actions = { test: '12345' };
      this.websocketsend(JSON.stringify(actions));
    },
    websocketonmessage(e) {
      console.log(e.data);
    },
    websocketonerror() {
      console.log('error');
    },
    websocketclose(e) {
      console.log('close connection', e);
    },
    websocketsend(Data) {
      this.websock.send(Data);
    },
  },
};
</script>
<style>
.text-box-text {
  padding: 8px 12px;
  max-width: 264px;
  font-size: 16px;
}
.text-box-text.custom {
  background-color: #d5d5d5;
  border-radius: 4px 24px 24px 24px;
  margin-right: 12px;
}
.text-box-text.main {
  background-color: #8d8d8d;
  border-radius: 24px 24px 4px 24px;
  margin-left: 12px;
}
.text-box-time {
  color: #979797;
  font-size: 12px;
}
.reverse {
  flex-direction: row-reverse;
}
.h-100 {
  height: 100%;
}
</style>
