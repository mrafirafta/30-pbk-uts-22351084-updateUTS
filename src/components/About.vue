<template>
  <div class="about-container">
    <div class="about-content">
      <h1 class="about-title">Tentang Alam</h1>
      <p class="about-description">"Dari pegunungan yang megah hingga lautan yang luas, keindahan alam menghadirkan ketenangan dan inspirasi yang tak terhingga bagi yang memandangnya."</p>
      <div class="about-features">
        <div class="feature">
          <i class="fas fa-users"></i>
          <p>Komunitas yang hangat</p>
        </div>
        <div class="feature">
          <i class="fas fa-lightbulb"></i>
          <p>Konten inovatif</p>
        </div>
        <div class="feature">
          <i class="fas fa-globe"></i>
          <p>Koneksi global</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  
}
</script>

<style>
/* Font Awesome Icons */
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css');

.about-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.7)), url('https://fotogambar.info/wp-content/uploads/2020/02/pemandangan-gunung-bromo-alam-indonesia.jpg');
  background-size: cover;
  background-position: center;
  position: relative;
  overflow: hidden; /* Hide overflow to enable parallax */
}

.about-content {
  text-align: center;
  color: #fcfcfc;
  padding: 60px;
  max-width: 800px;
  position: relative; /* Ensure z-index works correctly */
  z-index: 1; /* Ensure content stays above background */
}

.about-title {
  font-size: 3.5rem; /* Responsive font size */
  font-weight: bold;
  margin-bottom: 20px;
}

.about-description {
  font-size: 1.5rem; /* Responsive font size */
  line-height: 1.6;
  margin-bottom: 40px;
}

.about-features {
  display: flex;
  justify-content: center;
  gap: 30px;
}

.feature {
  text-align: center;
  width: 120px;
  transition: transform 0.3s ease;
}

.feature i {
  font-size: 3rem;
  margin-bottom: 10px;
}

.feature p {
  font-size: 1rem;
  margin: 0;
}

/* Hover effect */
.feature:hover {
  transform: translateY(-10px);
}

/* Parallax effect */
.about-container::before {
  content: "";
  background: url('https://fotogambar.info/wp-content/uploads/2020/02/pemandangan-gunung-bromo-alam-indonesia.jpg');
  background-size: cover;
  background-position: center;
  opacity: 0.6; /* Adjust opacity as needed */
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  position: absolute;
  z-index: -1; /* Behind content */
  transform: scale(1.2); /* Adjust scale for parallax effect */
}
</style>
