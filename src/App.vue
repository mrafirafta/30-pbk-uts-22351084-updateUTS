<template>
  <div>
    <header class="header">
      <nav class="menu">
        <ul>
          <li><router-link to="/"><i class="fas fa-home"></i> Home</router-link></li>
          <li><router-link to="/todos"><i class="fas fa-tasks"></i> Todos</router-link></li>
          <li><router-link to="/posts"><i class="fas fa-file-alt"></i> Posts</router-link></li>
          <li><router-link to="/albums"><i class="fas fa-images"></i> Albums</router-link></li>
          <li><router-link to="/about"><i class="fas fa-info-circle"></i> About</router-link></li>
        </ul>
      </nav>
    </header>
    <router-view></router-view>
  </div>
</template>

<script>
// Jangan lupa untuk menambahkan dependensi Font Awesome untuk ikon
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Poppins', sans-serif;
  background-color: #f0f4f8;
  color: #333;
}

.header {
  background: linear-gradient(135deg, #6c6c6c, #707070); /* Mengubah sudut gradien */
  padding: 20px;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.menu ul {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 0;
  margin: 0;
}

.menu ul li {
  margin: 0 15px;
  position: relative;
}

.menu ul li a {
  text-decoration: none;
  color: #f0f0f0;
  font-weight: bold;
  font-size: 1.2rem;
  padding: 10px 20px;
  border-radius: 20px;
  transition: background-color 0.3s, transform 0.3s;
  display: flex;
  align-items: center;
}

.menu ul li a i {
  margin-right: 8px;
}

.menu ul li a:hover {
  background-color: #255b7c; /* Warna latar belakang hover */
  transform: scale(1.05);
}

.menu ul li a::after {
  content: '';
  position: absolute;
  width: 0%;
  height: 2px;
  bottom: -5px;
  left: 50%;
  background-color: #f0f0f0;
  transition: width 0.3s, left 0.3s;
}

.menu ul li a:hover::after {
  width: 100%;
  left: 0;
}
</style>
