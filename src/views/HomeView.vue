<template>
  <div>
    <h1>Добро пожаловать!</h1>
    <p>Это главная страница нашего сайта</p>
    <router-link :to="{ name: 'about' }">О нас</router-link> |
    <router-link :to="{ name: 'contact' }">Контакты</router-link>
  </div>
</template>
