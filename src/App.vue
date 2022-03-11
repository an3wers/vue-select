<template>
  <div class="container">
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
    </nav>
    <router-view />
  </div>
</template>

<style>
nav {
  display: flex;
  padding: 1rem;
}

nav > a:not(:last-child) {
  margin-right: 1rem;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 0 0.5rem;
}
</style>
