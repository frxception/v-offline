<template>
  <div :class="{onlineClass: isOnline, offlineClass: !isOnline}">
    <slot
      v-if="isOnline"
      name="online"
    />
    <slot
      v-else
      name="offline"
    />
  </div>
</template>

<script>
const EVENTS = ['online', 'offline', 'load'];

export default {
  name: 'v-offline',
  props: {
    onlineClass: {
      type: String,
      required: false
    },
    offlineClass: {
      type: String,
      required: false
    }
  },
  data: () => ({
    isOnline: navigator.onLine || false,
  }),
  mounted() {
    EVENTS.forEach(event => window.addEventListener(event, this.updateOnlineStatus));
  },
  beforeDestroy() {
    EVENTS.forEach(event => window.removeEventListener(event, this.updateOnlineStatus));
  },
  methods: {
    updateOnlineStatus() {
      this.isOnline = navigator.onLine || false;
      this.$emit('detected-condition', this.isOnline);
    }
  }
};
</script>
