<template>
  <div class="container">
    <Navigation />
    <div>
      <RouterView />
    </div>
    <ContactDeleteModal />
    <NotificationContainer />
  </div>
</template>

<script setup>
  import Navigation from './components/Navigation.vue';
  import ContactDeleteModal from './components/ContactDeleteModal.vue';
  import NotificationContainer from './components/NotificationContainer.vue';

  import {RouterView} from 'vue-router';
  import {watch} from 'vue';
  import {storeToRefs} from 'pinia';
  import {useContactsStore} from './stores/contacts';

  const contactStore = useContactsStore();
  const {contacts} = storeToRefs(contactStore);

  watch(
    contacts,
    (val) => {
      localStorage.setItem('phonebook', JSON.stringify(val));
    },
    {deep: true}
  );
</script>

<style scoped></style>
