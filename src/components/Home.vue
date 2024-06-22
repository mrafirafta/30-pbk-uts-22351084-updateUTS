<template>
  <div class="home-container">
    <div class="background-overlay"></div>
    <div class="home-content">
      <h1 class="home-title">Eksplorasi Dunia Bersama Web Ini</h1>
      <router-link to="/about" class="btn" @click="handleAboutClick">Tentang Alam</router-link>
    </div>
    <div class="additional-box">
      <h2 class="box-title">UAS - PBK 223510845</h2>
      <p class="box-description">MUHAMMAD RAFI'I RAFTA</p>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    handleAboutClick() {
      this.$router.push('/about');
    }
  }
}
</script>

<style>
.home-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-image: url('https://image.winudf.com/v2/image/YXJ0Lm9jZWFubGl2ZXdhbGxwYXBlcl9zY3JlZW5fMV8ycWdtYXdmdw/screen-1.jpg?fakeurl=1&type=.jpg');
  background-size: cover;
  background-position: center;
  overflow: hidden;
}

.background-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.home-content {
  text-align: center;
  color: #fff;
  z-index: 1;
  max-width: 600px;
}

.home-title {
  font-size: 3.5rem;
  font-weight: bold;
  margin-bottom: 20px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.btn {
  padding: 12px 24px;
  background-color: #007bff;
  color: white;
  text-decoration: none;
  border-radius: 5px;
  transition: background-color 0.3s;
  font-size: 1.2rem;
  margin-top: 2000px;
}

.btn:hover {
  background-color: #0056b3;
}

.additional-box {
  background-color: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 20px;
  border-radius: 8px;
  margin-top: 30px;
  max-width: 400px;
  text-align: center;
  transition: background-color 0.3s, transform 0.3s;
}

.additional-box:hover {
  background-color: rgba(0, 0, 0, 0.9);
  transform: translateY(-5px);
}

.box-title {
  font-size: 2rem;
  font-weight: bold;
  margin-bottom: 10px;
}

.box-description {
  font-size: 1.2rem;
}
</style>
