<template>
  <v-app>
    <v-main>
      <v-container>
        <VCard>
          <VCardTitle>Todo List</VCardTitle>
          <VCardText>

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
              <VListItem v-for="item in list">
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
import { ref } from 'vue';


const list = ref<{ id: number, title: string, text: string }[]>([{
  id: 213,
  title: "Geschenke",
  text: "Blumen für Comdesk kaufen ",
}, {
  id: 529,
  title: "Software",
  text: "Updates installieren",
}]);

const title = ref('');
const text = ref('');


const addTodo = () => {
  list.value.push({
    id: Math.floor(Math.random() * 1000), //FIXME: might produce duplicate ids
    title: title.value,
    text: text.value,
  });
  title.value = '';
  text.value = '';
};
</script>
