<template>
    <div>
        <nav class="nav-bar">
            <div class="text-center">
                <div class="brand-name">
                    <img src="../assets/images/logo.png" alt="" class="brand-img">
                </div>
            </div>
        </nav>
    </div>
</template>


<script>
export default {
}

</script>

<style lang="scss" scoped>
@import '../assets/styles/_app.scss';
  .nav-bar {
      background-color: #666879;
      height: 60px;
      position: fixed;
      top:0;
      width: 100%;
      z-index: 1000;
      box-shadow:0 0 3px 1px #000;

    .brand-name {
        .brand-img {
            width:150px;
            padding-top: 15px;
        }

    }
  }
.logo {
      color: white;
      font-size: 30px;
      margin: 10px;
      font-weight: bold;
  }
  ul {
      float: right;
      margin: 20px;
  }
  ul li {
      margin: -10px;
  }
</style>