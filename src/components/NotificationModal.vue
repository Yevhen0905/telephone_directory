<template>
  <div class="notification_modal" :class="notificationClass">
    {{ notification.message }}
    <button class="notification_btn" @click="remove(notification)">&#x2715;</button>
  </div>
</template>

<script setup>
  import {ref, computed, onMounted, onUnmounted} from 'vue';
  import {useNotificationStore} from '../stores/notification';

  const props = defineProps({
    notification: {
      type: Object,
      required: true
    }
  });

  const timeOut = ref(null);

  const notificationStore = useNotificationStore();

  const remove = (notificationToRemove) => {
    notificationStore.remove(notificationToRemove);
  };

  const notificationClass = computed(() => {
    return `is_${props.notification.type}`;
  });

  onMounted(() => (timeOut.value = setTimeout(() => remove(props.notification), 5000)));
  onUnmounted(() => clearTimeout(timeOut.value));
</script>

<style lang="scss">
  .notification_modal {
    position: relative;
    width: 300px;
    margin: 10px 0;
    padding: 16px;
    border-radius: 10px;
    text-align: center;
    transition: all 0.3s ease;

    &.is_success {
      color: rgb(0 255 90 / 82%);
      background: rgb(86 224 109 / 31%);
    }

    &.is_danger {
      color: #ff00008c;
      background: rgb(224 105 86 / 31%);
    }
  }

  .notification_btn {
    position: absolute;
    top: 5px;
    right: 9px;
    outline: none;
    border: none;
    background: transparent;
    color: grey;
  }
</style>
