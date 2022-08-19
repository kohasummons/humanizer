<template>
  <div class="container">
    <div class="navbar">
        <div class="logo"><h1>Humanizer</h1><span>—generate human-readable ABI's</span></div>
        <nav>
            <a target="_blank" href="https://github.com/kohasummons/humanizer"><GithubIcon size="20" style="fill: #787878; cursor: pointer;" /></a>
            <a target="_blank" href="https://youtube.com/koha"><YoutubeIcon size="20" style="fill: #787878; cursor: pointer;" /></a>
            <a target="_blank" href="https://koha.hashnode.dev"><HashnodeIcon size="20" style="fill: #787878; cursor: pointer;" /></a>
        </nav>
    </div>
  </div>
</template>

<script setup>
import { defineComponent } from 'vue';
import { GithubIcon, YoutubeIcon, HashnodeIcon } from 'vue3-simple-icons'

defineComponent({
    name: "TheNavbar"
})

</script>

<style>
.navbar {
  display: flex;
  align-items: center;
  padding: 1rem 0;
}

.logo h1 {
  font-size: 1.2em;
  font-weight: 400;
  display: inline-block;
}

.logo{
  white-space: nowrap;
  width: 50%;
}

.logo span {
  font-size: 0.6em;
}

nav {
  margin-left: auto;
}

nav a {
  display: inline-block;
}

nav > * {
  margin-inline: .5em;
}

nav > *:last-of-type {
  margin-right: 0;
}

</style>