<script setup>
    import KanbanView from './components/KanbanView.vue';
</script>

<template>
  <header>
    <h1 class="centrar-texto"> Atefehs kanban</h1>
  </header>

  <main class="contenedor">
    <!-- import KabanView -->
    <KanbanView />
  </main>

</template>

<style scoped>
.centrar-texto {
    margin: 0;
    text-align: center;
}
</style>
