<template>
  <nav class="top-bar">
    <div class="menu">
      <button class="menu-button" v-on:click="
  menuView();
menuBTn();
      "></button>
    </div>
    <div class="logo">
      <router-link to="/" style="text-decoration: none; color: inherit">
        <h1>cronopédia.</h1>
      </router-link>
    </div>
    <div class="perfil">
      <section class="search">
        <input type="search" name="search-bar" id="search-bar-top" placeholder="Pesquise algo..."
          v-on:keyup.enter="search()" />
        <label for="search-bar" class="label-search"></label>

        <!-- remover a classe visible quando perfer o foco -->
        <section class="search-results-dropdown" data-visible="false">
          <section :key="resultado" v-for="resultado in resultados" :index="i">
            <route-link :to="`/artigos/${articleID}`" style="text-decoration: none; color: inherit">
              <h2>{{ resultado.titulo }}</h2>
              <p>{{ resultado.resumo }}</p>
            </route-link>
          </section>
        </section>
      </section>
      <button class="buttons btn-perfil">
        <i class="bx bxs-user-circle bx-sm"></i>
      </button>
    </div>
  </nav>
</template>

<script>

export default {
  asyncData() {
    return {
      resultados: []
    };
  },
  methods: {
    menuBTn() {
      const btn = document.querySelector("button.menu-button");
      btn.classList.contains("clicado")
        ? btn.classList.remove("clicado")
        : btn.classList.add("clicado");
    },

    menuView() {
      const primaryNav = document.querySelector("ul.primary-menu");
      var visi = primaryNav.getAttribute("data-visible");
      visi == "false"
        ? primaryNav.setAttribute("data-visible", "true")
        : primaryNav.setAttribute("data-visible", "false");
    },

    requisitar(q) {
      const response = await this.$axios.get(`/pagina/${q}`);
      this.resultados = response.data;

    },

    async search() {
      const search = document.querySelector(".search");
      const result_dropdown = document.querySelector(
        ".search-results-dropdown"
      );
      result_dropdown.innerHTML = "";
      const q = search.querySelector("input").value;

      this.requisitar(q);
    },
  },
};
</script>

<style scoped>
@import "~/static/css/form.css";
@import "~/static/css/geral.css";
@import "~/static/css/layout.css";
@import "~/static/css/menu.css";
@import "~/static/css/elements.css";
</style>

<style>
@import "~/static/css/top.css";
@import "~/static/css/input.css";
</style>