<template>
  <div id="capstone-app">
    <div id="nav" v-if="$store.state.token != ''">
      <router-link v-bind:to="{ name: 'home' }">View All</router-link>&nbsp;|&nbsp;
      <router-link v-bind:to="{ name: 'logout' }">Logout</router-link>
    </div>
    <router-view />
  </div>
</template>
