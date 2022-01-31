<template>
  <nav id="nav">
    <div class="nav__container">
        <router-link to="/" class="nav__title">
        {{
            userState
            ? userState.displayName || userState.email
            : 'Chat App'
        }}
        </router-link>
        <ul class="nav__menu">
            <li class="nav-item" v-if="!userState">
                <router-link to="/login" class="nav__menu__link">LogIn</router-link>
            </li>
            <li class="nav-item" v-if="!userState">
                <router-link to="/register" class="nav__menu__link">Register</router-link>
            </li>
            <li class="nav-item" v-if="userState">
                <router-link to="chat" class="btn-chat">
                    <span>Chat</span>
                </router-link>
            </li>
            <li class="nav-item" v-if="userState">
                <button @click="logout" class="btn btn-logout">Logout</button>
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
    padding: 14px 15px;
}
.nav__container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: 1040px;
    margin: auto;
}
.nav__title {
    font-size: 20px;
    color: #fff;
    text-decoration: none;
}
.nav__menu {
    display: flex;
    align-items: center;
    list-style: none;
    column-gap: 16px;
}
.nav__menu__link {
    color: #fff;
    text-decoration: none;
    font-weight: 300;
}
.btn-logout {
    padding: 8px 25px !important;
    background-image: linear-gradient(135deg, #f34079 40%, #fc894d) !important;
    margin-left: 15px;
    cursor: pointer;
    box-shadow: 0 0 10px #f44c72;
}
.btn-chat {
    width: 100px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    background-image: linear-gradient(144deg,#AF40FF, #5B42F3 50%,#00DDEB);
    border-radius: 20px;
    box-shadow: 0 0 10px #7a4bd8c7;
    margin-left: 7px;
}
.btn-chat span {
    display: flex;
    align-items: center;
    justify-content: center;
    color: #fff;
    text-decoration: none;
    background-color: #1f2633;
    position: absolute;
    width: calc(100% - 4px);
    height: calc(100% - 4px);
    inset: 0;
    border-radius: 20px;
    margin: auto;
    transition: 0.4s;
}
.btn-chat span:hover {
    background-color: transparent;
}
@media (max-width: 540px) {
    .nav__menu {
        display: none;
    }
}
</style>
