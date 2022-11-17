<script setup lang="ts">
import type { CatalogList } from '@/components/CatalogList/catalogList.interface';
import { reactive } from 'vue';
import styles from './CatalogList.module.scss';

type State = {
  catalogList: CatalogList[];
  text: string;
};

const state: State = reactive({
  catalogList: [
    { title: 'Catalog 1', id: 1, body: 'Catalog 1 body', userId: 1 },
    { title: 'Catalog 2', id: 2, body: 'Catalog 2 body', userId: 2 },
    { title: 'Catalog 3', id: 3, body: 'Catalog 3 body', userId: 3 },
    { title: 'Catalog 4', id: 4, body: 'Catalog 4 body', userId: 4 },
    { title: 'Catalog 5', id: 5, body: 'Catalog 5 body', userId: 5 },
    { title: 'Catalog 6', id: 6, body: 'Catalog 6 body', userId: 6 }
  ],
  text: ''
});

const addCatalog = () => {
  state.catalogList.push({
    title: state.text,
    id: state.catalogList.length + 1,
    body: 'Catalog body',
    userId: state.catalogList.length + 1
  });
  state.text = '';
};

const onSubmit = () => addCatalog();

const removeCatalog = (id: number) => {
  state.catalogList = state.catalogList.filter((item) => item.id !== id);
};
</script>

<template>
  <div :class="styles.wrapper">
    <h1 :class="styles.title">Add Post</h1>
    <form @submit.prevent="onSubmit">
      <input v-model="state.text" :class="styles.input" />
      <button type="submit">Add Catalog</button>
    </form>
    <div v-if="!state.catalogList.length">Catalogs not found!</div>
    <ul v-else>
      <li
        v-for="catalog in state.catalogList"
        :key="catalog.id"
        :class="styles.catalog"
        @click="removeCatalog(catalog.id)"
      >
        <span
          >{{ catalog.id }} - {{ catalog.title }}, desc:
          {{ catalog.body }}</span
        >
      </li>
    </ul>
  </div>
</template>
