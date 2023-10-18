<template>
    <footer class="bg-light py-4">
      <div class="container">
        <div class="row">
          <div class="col-md-6">
            <p>&copy; 2023 Aldi Misbahul Kabir</p>
          </div>
          <div class="col-md-6 text-md-right">
            <a href="#">Privacy Policy</a> | <a href="#">Terms of Service</a>
          </div>
        </div>
      </div>
    </footer>
  </template>
  
  <script>
  export default {
    data() {
      return {
        currentYear: new Date().getFullYear,
      };
    },
  };
  </script>
  
  <style scoped>
  /* Add custom CSS styles if needed */
  </style>