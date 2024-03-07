<template>
  <v-data-table-virtual class="sticky-header-table"
    :headers="headers"
    :items="creadores"
    item-value="Autor"
  ></v-data-table-virtual>
</template>

<script setup>
import { ref, onMounted } from "vue";

const urlPosts = "https://jsonplaceholder.typicode.com/posts";
const urlAutores = "https://jsonplaceholder.typicode.com/users";
const creadores = ref([]);

onMounted(async () => {
  try {
    // Obtener datos de los posts
    const responsePosts = await fetch(urlPosts);
    const dataPosts = await responsePosts.json();

    // Obtener datos de los autores
    const responseAutores = await fetch(urlAutores);
    const dataAutores = await responseAutores.json();

    // Combinar los datos de autores y posts
    const mergedData = dataPosts.map(post => {
      const autor = dataAutores.find(autor => autor.id === post.userId);
      return {
        Autor: autor.name,
        Titulo: post.title,
        Post: post.body
      };
    });

    // Actualizar la variable creadores con los datos combinados
    creadores.value = mergedData;
  } catch (error) {
    console.error("Error al obtener los datos:", error);
  }
});

const headers = [
  { title: "Autor", align: "center", key: "Autor" },
  { title: "Título", align: "center", key: "Titulo" },
  { title: "Post", align: "center", key: "Post" }
];

</script>
