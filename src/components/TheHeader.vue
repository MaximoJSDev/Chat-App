<template>
  <header class="header">
    <div class="header__container">
      <h1 class="header__title">
        Chat App
      </h1>
      <div class="header__profile" v-if="userState">
        <h6 class="header__profile__title">Maximo 4ever</h6>
        <div class="btn-nav profile">
          <img src="../assets/Profile.svg" alt="Profile icon" title="Profile">
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  props: ['userState']
}
</script>

<style>
.header {
  width: calc(100vw - 78px);
  height: 69px;
  background-color: #1f2633;
  color: #fff;
  display: flex;
  align-items: center;
  grid-area: head;
}
.header__container {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 40px;
}
.header__title {
  font-size: 26px;
}
.header__profile {
  height: 49px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 9px;
  column-gap: 20px;
}
.header__profile__title {
  font-size: 21px;
  font-weight: 400;
}
</style>
