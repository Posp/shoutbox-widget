<style lang="scss" src="./styles/ShMessageAdmin.scss"></style>
<template lang="html" src="./templates/ShMessageAdmin.html"></template>
<script>
import ShSubmit from './ShSubmit'

export default {
  components: {
    ShSubmit
  },
  props: ['data', 'socket', 'user', 'stream', 'renewCsrf', 'type'],
  data () {
    return {
      showReply: this.showReply || false,
      toggle: this.toggle || false
    }
  },
  methods: {
    getSocket () {
      return this.socket
    },
    review () {
      this.socket.put(`/messages/${this.data.id}`, {
        id: this.data.id,
        reviewed: true,
        _csrf: window.CSRF
      })
      this.toggle = false
    },
    reject () {
      this.socket.put(`/messages/${this.data.id}`, {
        id: this.data.id,
        reviewed: true,
        published: false,
        _csrf: window.CSRF
      })
      this.toggle = false
    },
    publish () {
      this.socket.put(`/messages/${this.data.id}`, {
        id: this.data.id,
        published: true,
        reviewed: true,
        _csrf: window.CSRF
      })
      this.toggle = false
    },
    unpublish () {
      this.socket.put(`/messages/${this.data.id}`, {
        id: this.data.id,
        published: false,
        reviewed: true,
        _csrf: window.CSRF
      })
      this.toggle = false
    },
    remove () {
      // DELETE doesn't work in Fx and maybe other browsers
      this.socket.get(`/messages/destroy/${this.data.id}`, {
        _csrf: window.CSRF
      })
      this.toggle = false
    },
    toggleFloat () {
      this.toggle = !this.toggle
    }
  }
}
</script>
