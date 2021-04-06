<template>
  <section>
    <div v-if="produto" class="produto">
      <ul class="fotos" v-if="produto.fotos">
        <li v-for="(foto, index) in produto" :key="index">
          <img :src="fotos.src" :alt="fotos.titulo" />
        </li>
      </ul>
      <div class="info">
        <h1>{{ produto.nome }}</h1>
        <p class="preco">{{ produto.preco | precoFormat }}</p>
        <p class="descricao">{{ produto.descricao }}</p>
        <button class="btn" v-if="produto.vendido === 'false'">Comprar</button>
        <button class="btn" disabled v-else>Produto Vendido</button>
      </div>
    </div>
    <PaginaCarregando v-else />
  </section>
</template>

<script>
import { api } from "../services.js";
export default {
  name: "Produto",
  props: ["id"],
  data() {
    return {
      produto: null,
    };
  },
  methods: {
    getProdutos() {
      api.get(`/produto/${this.id}`).then((response) => {
        this.produto = response.data;
      });
    },
  },
  created() {
    this.getProdutos();
  },
};
</script>

<style scoped>
.produto {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 30px;
  max-width: 900px;
  padding: 60px 20px;
  margin: 0 auto;
}

.preco {
  color: #e80;
  font-weight: bold;
  font-size: 1.5rem;
  margin-bottom: 40px;
}

.descricao {
  font-size: 1.2rem;
}

.btn {
  margin-top: 60px;
  width: 200px;
}
</style>
