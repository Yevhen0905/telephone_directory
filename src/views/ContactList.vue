<template>
  <div class="contact_list">
    <div v-if="contacts.length > 0" class="wrapper_contact">
      <h1 class="contact_title">Contact List</h1>
      <div class="contact_action">
        <div class="contact_input">
          <label class="contact_label">Search</label>
          <input
            class="input"
            type="text"
            placeholder="search by name or telephone..."
            v-model="search"
          />
        </div>
        <div class="contact_input">
          <label class="contact_label">Sorting</label>
          <select class="input select" v-model="sortOrder">
            <option
              class="select_option"
              v-for="option in sortOptions"
              :key="option"
              :value="option"
            >
              {{ option }}
            </option>
          </select>
        </div>
        <div class="contact_input contact_input_radio">
          <div class="contact_label">Show</div>
          <div class="wrapper_radio">
            <label class="label_radio">
              <input class="radio" type="radio" value="all" v-model="favoritesToggle" />
              All
            </label>
            <label class="label_radio">
              <input class="radio" type="radio" value="favorites" v-model="favoritesToggle" />
              Favorites
            </label>
          </div>
        </div>
      </div>
      <div v-if="currentList.length > 0">
        <ContactCard v-for="contact in currentList" :key="contact.id" :contact="contact" />
      </div>
      <div v-else class="no_contact">No contacts found.</div>
      <!-- <div class="bg_img_list"></div> -->
    </div>
    <div v-else class="no_contact">
      <p class="no_contact_text">No contacts. Click the Add contact button above to add one.</p>
      <div class="bg_img"></div>
    </div>
  </div>
</template>

<script setup>
  import ContactCard from '../components/ContactCard.vue'

  import { ref, computed } from 'vue'
  import { useContactsStore } from '@/stores/contacts'
  import { useSearch } from '@/composables/useSearch'
  import { useSortingList } from '@/composables/useSortingList'
  import { useFavorites } from '@/composables/useFavorites'

  const contactStore = useContactsStore()
  contactStore.fetchContact()

  const contacts = computed(() => contactStore.contacts)

  const { search, filteredContacts } = useSearch(contacts)

  const { sortOrder, sortOptions, currentSortList } = useSortingList(filteredContacts)

  const favoritesToggle = ref('all')
  const { currentList } = useFavorites(favoritesToggle, currentSortList)
</script>

<style lang="scss">
  .contact_action {
    display: flex;
    gap: 25px;

    @media only screen and (max-width: 800px) {
      gap: 5px;
    }

    @media only screen and (max-width: 590px) {
      flex-direction: column;
    }
  }

  .select {
    padding: 7px 10px;
  }

  .wrapper_contact {
    padding-bottom: 15px;
  }

  .wrapper_radio {
    display: flex;
    gap: 14px;
  }

  .label_radio {
    display: flex;
    font-size: 16px;
    align-items: center;
    gap: 3px;
  }

  .radio {
    width: 20px;
    height: 20px;
  }

  .no_contact {
    text-align: center;
    padding-top: 50px;
    font-size: 27px;
  }

  .bg_img_list {
    background-image: url('../assets/img/book-159880_1280.webp');
    background-size: cover;
    background-position: left top;
    background-repeat: no-repeat;
    position: absolute;
    z-index: -1;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    &::before {
      content: '';
      backdrop-filter: blur(10px);
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
    }
  }

  .bg_img {
    background-image: url('../assets/img/black-iphone-5-3.jpg');
    background-size: cover;
    background-position: left top;
    background-repeat: no-repeat;
    position: absolute;
    z-index: -1;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    &::before {
      content: '';
      backdrop-filter: blur(6px);
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
    }
  }

  .no_contact_text {
    font-size: 35px;
    background: linear-gradient(45deg, #0720a1, #201b20 27%, #50390f);
    color: #ebeef4;
    -webkit-background-clip: text;
    -webkit-text-stroke: 3px transparent;
    text-shadow: 4px 1px 2px #230fcb;
  }

  .contact_input_radio {
    margin-left: 10px;
  }
</style>
