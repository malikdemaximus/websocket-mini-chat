<template>
  <div>
    <section>
  <div class="container py-5">

    <div class="row d-flex justify-content-center">
      <div class="col-md-10 col-lg-8 col-xl-6">

        <div class="card" id="chat2">
          <div class="card-header d-flex justify-content-between align-items-center p-3">
            <h5 class="mb-0">Chat</h5>
            <button  type="button" data-mdb-button-init data-mdb-ripple-init class="btn btn-primary btn-sm" data-mdb-ripple-color="dark">Let's Chat
              App</button>
          </div>
          <div class="card-body" data-mdb-perfect-scrollbar-init style="position: relative; height: 400px">
          <div v-for="(item, ind) in chat" :key="ind">
            <div v-if="item.sender == 2" class="d-flex flex-row justify-content-start">
            <p class="small p-2 me-3 mb-1 text-white rounded-3 bg-primary">{{item.text}}</p>
            <img src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-chat/ava4-bg.webp"
                alt="avatar 1" style="width: 45px; height: 100%;">
              <div>
                
                <p class="small ms-3 mb-3 rounded-3 text-muted">23:58</p>
              </div>
            </div>

            <div v-else class="d-flex flex-row justify-content-end mb-4 pt-1">
              <div>
                <p class="small p-2 me-3 mb-1 text-white rounded-3 bg-primary">{{item.text}}</p>
                <p class="small me-3 mb-3 rounded-3 text-muted d-flex justify-content-end">00:06</p>
              </div>
              <img src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-chat/ava3-bg.webp"
                alt="avatar 1" style="width: 45px; height: 100%;">
            </div>
        </div>
          </div>
          <div class="card-footer text-muted d-flex justify-content-start align-items-center p-3">
            <img src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-chat/ava3-bg.webp"
              alt="avatar 3" style="width: 40px; height: 100%;">
            <input type="text" class="form-control form-control-lg" v-model="message" @keyup.enter="sendMessage"
              placeholder="Type message" data-testid="message-input">
              <button @click="sendMessage" class="ml-2 btn btn-primary" data-testid="send-button">Send</button>
          </div>
        </div>
<textarea data-testid="chat-output" class="small p-2 ms-3 mb-1 rounded-3 bg-body-tertiary">
                {{response}}</textarea>
      </div>
    </div>

  </div>
</section>
  </div>
</template>

<script>
export default {
  name: "Chat",
  data() {
    return {
      message: null,
      response: null,
      chat: [],
    }
  },
  mounted() {
    console.log("Starting connection to WebSocket Server")
    this.connection = new WebSocket("wss://echo.websocket.org")

    this.connection.onmessage = (event) => {
      this.chat.push({
        sender: 2,
        text: JSON.stringify(event),
      })
      this.response = JSON.stringify(event);
    }

    this.connection.onopen = function(event) {
      console.log(event)
      console.log("Successfully connected to the echo websocket server...")
    }
  },
  beforeUnmounted() {
    this.connection.close();
    this.connection = null;
  },
   methods: {
    sendMessage() {
      this.connection.send(this.message);
      this.chat.push({
        sender: 1,
        text: this.message,
      });
      this.message = '';
    }
  },
};
</script>

<style scoped lang="scss">
@import 'https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css';
.card-body {
  overflow: auto;
}
#chat2 .form-control {
border-color: transparent;
}

#chat2 .form-control:focus {
border-color: transparent;
box-shadow: inset 0px 0px 0px 1px transparent;
}

.divider:after,
.divider:before {
content: "";
flex: 1;
height: 1px;
background: #eee;
}
</style>
