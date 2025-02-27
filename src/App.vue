<template>
  <v-app>
    <v-main>
      <v-container>
        <VCard>
          <VCardTitle>Todo List</VCardTitle>
          <VCardText>
            <!-- might use v-data-table, but list is prettier for now-->
            <!--<v-data-table :items="list"></v-data-table>-->
            <VSelect v-model="currentCategory" :items="CATEGORIES" label="Category" />
            <VList>
              <VListItem>
                <VAlert v-if="list.length === 0" color="info">No items yet</VAlert>
                <VRow v-else>
                  <VCol cols=4>
                    <b>ID</b>
                  </VCol>
                  <VCol cols=4>
                    <b>Title</b>
                  </VCol>
                  <VCol cols=4>
                    <b>Text</b>
                  </VCol>
                </VRow>
              </VListItem>
              <VListItem v-for="item in list.sort((a, b) => a.id - b.id).filter(i => i.category === currentCategory)">
                <VRow>
                  <VCol cols=4>
                    <VListItemSubtitle class="text--muted">{{ `#${item.id}` }}</VListItemSubtitle>
                  </VCol>
                  <VCol cols=4>
                    <VListItemTitle>{{ item.title }}</VListItemTitle>
                  </VCol>
                  <VCol cols=4>
                    <VListItemSubtitle>{{ item.text }}</VListItemSubtitle>
                  </VCol>
                </VRow>
              </VListItem>
            </VList>
          </VCardText>
        </VCard>

        <VCard class="mt-4">
          <VCardTitle>Add Todo Item</VCardTitle>
          <VCardText>
            <VForm @submit.prevent="addTodo">
              <VTextField label="Title" v-model="title" />
              <VTextField label="Text" v-model="text" />
              <VBtn color="primary" type="submit">Add</VBtn>
            </VForm>
          </VCardText>
        </VCard>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
import { useLocalStorage } from '@vueuse/core';
import { ref } from 'vue';

const CATEGORIES = ["Allgemein", "Software", "Hardware"] as const;

const currentCategory = ref<typeof CATEGORIES[number]>("Allgemein");

const list = useLocalStorage<{ id: number, title: string, text: string, category: typeof CATEGORIES[number] }[]>("todo-storage", [{
  id: 213,
  title: "Geschenke",
  text: "Blumen für Comdesk kaufen ",
  category: 'Allgemein'
}, {
  id: 529,
  title: "Software",
  text: "Updates installieren",
  category: 'Allgemein'
}]);

const title = ref('');
const text = ref('');

function addTodo() {
  list.value.push({
    id: Math.floor(Math.random() * 1000), //FIXME: might produce duplicate ids
    title: title.value,
    text: text.value,
    category: currentCategory.value
  });

  title.value = '';
  text.value = '';
};
</script>
