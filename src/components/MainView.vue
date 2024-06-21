<template>
    <div id="app">
      <h1>Vue 2 vs React</h1>
      <div class="card-container">
        <ComparisonCard 
          v-for="(difference, index) in differences" 
          :key="index" 
          :title="difference.title" 
          :description="difference.description" 
        />
      </div>

    </div>
  </template>
  
  <script>
  import ComparisonCard from './ComparisonCard.vue';
  
  export default {
    name: 'App',
    components: {
      ComparisonCard
    },
    data() {
      return {
        differences: [
          {
            title: "Enfoque en la biblioteca",
            description: "Vue es un framework completo, mientras que React es una biblioteca centrada en la vista."
          },
          {
            title: "Data Binding",
            description: "Vue usa un binding bidireccional, mientras que React usa un binding unidireccional."
          },
          {
            title: "Virtual DOM",
            description: "Ambos usan Virtual DOM, pero la implementación y optimización difiere entre ellos."
          },
          {
            title: "Manejo de estado",
            description: "React usa principalmente Redux o Context API, mientras que Vue usa Vuex."
          }
        ]
      }
    }
  }
  </script>
  
  <style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .card-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  </style>
  