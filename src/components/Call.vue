<template>
    <div>
        <video ref="localVideo" autoplay></video>
        <video ref="remoteVideo" autoplay></video>
    </div>
</template>

<script>
//import SimplePeer from 'simple-peer';

export default {
  data() {
    return {
      localStream: null,
      remoteStream: null,
      peer: null
    };
  },
  mounted() {
    this.startLocalVideo();
    this.createPeer();
  },
  methods: {
    async startLocalVideo() {
      try {
        this.localStream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
        this.$refs.localVideo.srcObject = this.localStream;
      } catch (error) {
        console.error('Error accessing webcam:', error);
      }
    },
    createPeer() {
      this.peer = new SimplePeer({
        initiator: true,
        stream: this.localStream,
      });

      this.peer.on('signal', signal => {
        // Send signal to other peer via signaling server
      });

      this.peer.on('stream', stream => {
        this.remoteStream = stream;
        this.$refs.remoteVideo.srcObject = this.remoteStream;
      });

      // Handle ICE candidates, data channel, etc.
    }
  }
};
</script>

<style scoped>
    /* Add your styles here */
</style>