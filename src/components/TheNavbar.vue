<template>
  <nav id="nav">
    <div class="nav__container">
        <div class="ico">
            <img src="../assets/logo.png" alt="Logout icon" title="Logout" />
        </div>
        <ul class="nav__menu">
            <li class="nav-item">
                <router-link to="/" class="btn-nav home">
                    <img src="../assets/Home.svg" alt="Home icon" title="Home">
                </router-link>
            </li>
            <li class="nav-item" v-if="!userState">
                <router-link to="/login" class="btn-nav chat">
                    <img src="../assets/Login.svg" alt="Login icon" title="Login">
                </router-link>
            </li>
            <li class="nav-item" v-if="userState">
                <router-link to="/chat" class="btn-nav chat">
                    <img src="../assets/Chat.svg" alt="Chat icon" title="Chat">
                </router-link>
            </li>
            <li class="nav-item" v-if="userState">
                <div class="btn-nav logout">
                    <img src="../assets/Logout.svg" alt="Logout icon" @click="logout" class="btn-logout" title="Logout" />
                </div>
            </li>
        </ul>
        <div class="ico dark-light">
            <img src="../assets/Moon.svg" alt="Logout icon" title="Logout" />
            <img src="../assets/Sun.svg" alt="Logout icon" title="Logout" />
        </div>
    </div>
  </nav>
</template>

<script>
export default {
  props: ['userState', 'logout']
}
</script>

<style>
#nav {
    display: flex;
    background-color: #1f2633;
    color: #fff;
    height: 100%;
    grid-area: nav;
}
.nav__container {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    /* max-width: 1040px; */
    padding: 17px 14px 36px;
    height: 100%;
    margin: auto;
}
.nav__menu {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    list-style: none;
    row-gap: 40px;
}
.btn-nav img {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    width: 50px;
    padding: 10px;
    border-radius: 50%;
    object-fit: cover;
    cursor: pointer;
}
.ico {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}
.dark-light {
    row-gap: 50px;
    padding: 14px 8px;
    border-radius: 35px;
    background: #1a1e25;
    outline: 1px solid #6f13f5;
}
.dark-light img {
    padding: 4px;
    cursor: pointer;
}
.ico img {
    height: 34px;
}
/* Background */
.profile img {
    background-image: linear-gradient(-225deg, #AC32E4 0%, #7918F2 48%, #4801FF 100%);
    box-shadow: 0 0 25px #735fd5ad;
}
.home img {
    background-image: linear-gradient(144deg,#AF40FF, #5B42F3 50%,#00DDEB);
    box-shadow: 0 0 15px #7a4bd8c7;
}
.chat img {
    background-image: linear-gradient(315deg,#39c400,#00c7ff);
    box-shadow: 0 0 15px #1fce80;
}
.logout img {
    background-image: linear-gradient(135deg, #f34079 40%, #fc894d);
    box-shadow: 0 0 15px #f44c72;
}
</style>
