<template>
  <nav id="nav">
    <div class="nav__container">
        <a href="#!" class="btn-nav profile">
            <img src="../assets/Profile.svg" alt="Profile svg" title="Profile">
        </a>
        <ul class="nav__menu">
            <li class="nav-item">
                <router-link to="/" class="btn-nav home">
                    <img src="../assets/Home.svg" alt="Home svg" title="Home">
                </router-link>
            </li>
            <li class="nav-item" v-if="!userState">
                <router-link to="/chat" class="btn-nav chat">
                    <img src="../assets/Login.svg" alt="Login svg" title="Login">
                </router-link>
            </li>
            <li class="nav-item" v-if="userState">
                <router-link to="/chat" class="btn-nav chat">
                    <img src="../assets/Chat.svg" alt="Chat svg" title="Chat">
                </router-link>
            </li>
            <li class="nav-item" v-if="userState">
                <a href="#!" class="btn-nav logout">
                    <img src="../assets/Logout.svg" alt="Logout svg" @click="logout" class="btn-logout" title="Logout" />
                </a>
            </li>
        </ul>
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
    background-color: #1f2633;
    color: #fff;
    height: 100%;
}
.nav__container {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    /* max-width: 1040px; */
    padding: 40px 14px;
    margin: auto;
}
.nav__menu {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    list-style: none;
    row-gap: 30px;
}
.btn-nav img {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    padding: 10px;
    border-radius: 50%;
    object-fit: cover;
}
.profile {
    margin-bottom: 50px;
    padding-bottom: 50px;
    border-bottom: 2px solid #343d4d;
}
/* Background */
.profile img {
    background-image: linear-gradient(315deg,#8243ff,#ff0032);
    box-shadow: 0 0 25px #cc1b85;
}
.home img {
    background-image: linear-gradient(144deg,#AF40FF, #5B42F3 50%,#00DDEB);
    box-shadow: 0 0 15px #7a4bd8c7;
}
.chat img {
    background-image: linear-gradient(315deg,#3ed500,#00c7ff);
    box-shadow: 0 0 15px #1fce80;
}
.logout img {
    background-image: linear-gradient(135deg, #f34079 40%, #fc894d);
    box-shadow: 0 0 15px #f44c72;
}
</style>
