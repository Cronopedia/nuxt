<template>
  <section class="body">
    <TopBar></TopBar>
    <MobileChange></MobileChange>
    <PrimaryMenu></PrimaryMenu>

    <section class="article-view" :id="`${article.id}`">
      <section class="article-content">
        <form>
          <editor
          api-key="u19n8hdzwus3tpuaj01ao7t5z5jtwdvxyaouzt77iy5hn75j"
          :init="{
            height: 700,
            skin: 'naked',
            icons: 'thin',
            plugins: [
              'a11ychecker',
              'advlist',
              'advcode',
              'advtable',
              'autolink',
              'checklist',
              'export',
              'lists',
              'link',
              'image',
              'charmap',
              'preview',
              'anchor',
              'searchreplace',
              'powerpaste',
              'fullscreen',
              'formatpainter',
              'media',
              'table',
              'help',
              'wordcount',
            ],
            // Contexto (click no botão direito)
            contextmenu: 'link image table',

            // Contexto (seleção de conteudo)
            setup: (editor) => {
              editor.ui.registry.addContextToolbar('textselection', {
                predicate: (node) => !editor.selection.isCollapsed(),
                items: 'bold italic | blockquote',
                position: 'selection',
                scope: 'node',
              });
            },

            // Barra de Ferramentas
            toolbar:
              'undo redo fontselect fontsizeselect formatting aligns \
                list  outdent indent removeformat  a11ycheck  cancel save',

            // Grupos de ferramentas
            toolbar_groups: {
              formatting: {
                icon: 'format',
                tooltip: 'Format',
                items: 'bold italic underline | superscript subscript',
              },
              aligns: {
                icon: 'align-center',
                tooltip: 'Align',
                items: 'alignleft aligncenter alignright alignjustify',
              },
              list: {
                icon: 'unordered-list',
                tooltip: 'List',
                items: 'bullist numlist checklist',
              },
            },

            // Menu
            // menubar: 'edit insert format table tools help',
            menubar: false,

            // Mobile
            mobile: {
              // Contexto (seleção de conteudo)
              setup: (editor) => {
                editor.ui.registry.addContextToolbar('textselection', {
                  predicate: (node) => !editor.selection.isCollapsed(),
                  items: 'bold italic | blockquote',
                  position: 'selection',
                  scope: 'node',
                });
              },
            },
          }"
          :initial-value="`${article.conteudo}`"
          output-format="html"
        />

        <button @click="salvar()" >Salvar</button>
        </form>
      </section>
      <section class="article-imagens">
        <ArticleImage
          :key="i"
          v-for="i in article.imagens"
          :url="i"
        ></ArticleImage>
      </section>
    </section>
  </section>
</template>

<script>
import tinyEditor from "@tinymce/tinymce-vue";
import MobileChange from "~/components/MobileChange.vue";

export default {
  name: "EdicaoArtigo",
  components: {
    editor: tinyEditor,
    MobileChange,
  },
  head() {
    return {
      title: "Editando Artigo",
    };
  },
  async asyncData({ params, $axios }) {
    const article = await $axios.$get("/paginas/id/"+params.key);
    console.log(article);
    return { article };
  },
  methods: {
    async salvar(){
      var textarea = document.querySelector('div.tox-sidebar-wrap div.tox-edit-area iframe').contentDocument;
      var mudanca = textarea.querySelector('body p').innerText;
      const a = await $axios.$put("/paginas/atualizar/1/"+ article.id, { mudanca});
    }
  },
};
</script>

<style>
@import "~/static/css/article.css";
</style>


<!-- 
  Backup

            <editor
          api-key="u19n8hdzwus3tpuaj01ao7t5z5jtwdvxyaouzt77iy5hn75j"
          :init="{
            height: 700,
            skin: 'naked',
            icons: 'thin',
            plugins: [
              'a11ychecker',
              'advlist',
              'advcode',
              'advtable',
              'autolink',
              'checklist',
              'export',
              'lists',
              'link',
              'image',
              'charmap',
              'preview',
              'anchor',
              'searchreplace',
              'powerpaste',
              'fullscreen',
              'formatpainter',
              'media',
              'table',
              'help',
              'wordcount',
            ],
            // Contexto (click no botão direito)
            contextmenu: 'link image table',

            // Contexto (seleção de conteudo)
            setup: (editor) => {
              editor.ui.registry.addContextToolbar('textselection', {
                predicate: (node) => !editor.selection.isCollapsed(),
                items: 'bold italic | blockquote',
                position: 'selection',
                scope: 'node',
              });
            },

            // Barra de Ferramentas
            toolbar:
              'undo redo fontselect fontsizeselect formatting aligns \
                list  outdent indent removeformat  a11ycheck  cancel save',

            // Grupos de ferramentas
            toolbar_groups: {
              formatting: {
                icon: 'format',
                tooltip: 'Format',
                items: 'bold italic underline | superscript subscript',
              },
              aligns: {
                icon: 'align-center',
                tooltip: 'Align',
                items: 'alignleft aligncenter alignright alignjustify',
              },
              list: {
                icon: 'unordered-list',
                tooltip: 'List',
                items: 'bullist numlist checklist',
              },
            },

            // Menu
            // menubar: 'edit insert format table tools help',
            menubar: false,

            // Mobile
            mobile: {
              // Contexto (seleção de conteudo)
              setup: (editor) => {
                editor.ui.registry.addContextToolbar('textselection', {
                  predicate: (node) => !editor.selection.isCollapsed(),
                  items: 'bold italic | blockquote',
                  position: 'selection',
                  scope: 'node',
                });
              },
            },
          }"
          :initial-value="`${article.conteudo}`"
          output-format="html"
        />


 -->