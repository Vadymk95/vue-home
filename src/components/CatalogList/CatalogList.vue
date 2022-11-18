<script setup lang="ts">
import type { CatalogList } from '@/components/CatalogList/catalogList.interface';
import { reactive } from 'vue';
import { useQuery } from 'vue-query';
import axios from 'axios';
import styles from './CatalogList.module.scss';

type State = {
  catalogList: CatalogList[];
  text: string;
};

const getCatalogs =
  'https://jsonplaceholder.typicode.com/posts?_page=0&_limit=10';

const fetcher = async (): Promise<CatalogList[]> =>
  await axios.get(getCatalogs).then((res) => (state.catalogList = res.data));

const { isLoading } = useQuery('catalogList', fetcher);

const state: State = reactive({
  catalogList: [],
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
    <div v-if="isLoading">Loading...</div>
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
