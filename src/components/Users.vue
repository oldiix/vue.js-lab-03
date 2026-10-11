<script setup lang="ts">
import UserCard from '@/components/UserCard.vue'
import {user} from '@/data/users'
</script>

<template>
  <section class="users">
    <UserCard :user="user" />
  </section>
</template>
