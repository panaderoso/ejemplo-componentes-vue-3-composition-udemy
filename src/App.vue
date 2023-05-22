<script setup>
import BlogPost from "./components/BlogPost.vue";
import { ref, computed, onMounted } from "vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";

/*const posts = ref([
  {title: 'Post 1', id: 1, body: 'descripción 1',colorText:"primary"},
  {title: 'Post 2', id: 2, body: 'descripción 2',colorText:"secondary"},
  {title: 'Post 3', id: 3, body: 'descripción 3',colorText:"success"},
  {title: 'Post 4', id: 4},

]);*/

const posts = ref([]);
const favorito = ref("");
const inicio = ref(0);
const postXpage = 10;
const fin = ref(postXpage);
const maxLength = ref(0);
const maxLength2 = computed(() => posts.value.length);
const loading = ref(false);

const next = () => {
  inicio.value = inicio.value + postXpage;
  fin.value = fin.value + postXpage;
};
const prev = () => {
  inicio.value -= postXpage;
  fin.value -= postXpage;
};

const cambiarFavorito = (postiel) => {
  favorito.value = postiel;
};

//(loading.value = false));
//.then((data)=> console.log(data))

//el onmounted espera a que termine a cargar el componente , se usa cuando queremos utilizar algo que ya se haya
//montado en el arbol DOM del template
//SE PUEDE HACER EL TRY Y CATCH SIN EL ONMUNTED
onMounted(async () => {
  fetchData();
});

const fetchData = async () => {
  loading.value = true;
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    console.log(error);
  } finally {
    setTimeout(() => {
      loading.value = false;
    }, 2000);
  }
};

//el fetch sin onmounted lo crea en el create del componente (MEJOR OPCIÓN)
/* fetch("https://jsonplaceholder.typicode.com/posts")
    .then((res) => res.json())
    .then((data) => (posts.value = data))
    .finally(() => {
      setTimeout(() => {
        loading.value = false;
      }, 2000);
    });*/
</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mis Post Favorito: {{ favorito }}</h2>

    <!-- <button @click="next">Next Provisorio</button>-->

    <PaginatePost
      @next="next"
      @prev="prev"
      :inicio="inicio"
      :fin="fin"
      :maxLength="maxLength2"
    />

    <BlogPost
      v-for="post in posts.slice(inicio, fin)"
      :userId="post.id"
      :title="post.title"
      :id="post.id"
      :body="post.body"
      :colorText="post.colorText"
      @cambiarFavoritoNombre="cambiarFavorito"
    />

    <!--<BlogPost title="Post 02" :id="2" body="Description 2" colorText="secondary"/>
  <BlogPost title="Post 03" :id="3" body="Description 3" colorText="success"/>-->
  </div>
</template>
