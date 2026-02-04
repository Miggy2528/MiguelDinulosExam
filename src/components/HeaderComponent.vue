<template>
  <header class="header">
    <h1>{{ title }}</h1>
    <nav class="navigation">
      <router-link to="/">Home</router-link>
      <router-link to="/students">Students</router-link>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'HeaderComponent',
  props: {
    title: {
      type: String,
      default: 'Student Info App'
    }
  }
}
</script>

<style scoped>
.header {
  background-color: #42b983;
  padding: 20px;
  color: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.header h1 {
  margin: 0 0 15px 0;
  font-size: 2rem;
}

.navigation {
  display: flex;
  gap: 20px;
  justify-content: center;
}

.navigation a {
  color: white;
  text-decoration: none;
  padding: 8px 16px;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.navigation a:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

.navigation a.router-link-active {
  background-color: rgba(255, 255, 255, 0.3);
}
</style>