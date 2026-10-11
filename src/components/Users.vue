<script setup lang="ts">
import {ref} from 'vue'
import UserCard from '@/components/UserCard.vue'
import {users as usersData} from '@/data/users'
import type {User} from '@/types/user'

const users = ref<User[]>(usersData)
</script>

<template>
  <section class="users">
    <div class="users__list">
      <UserCard
        v-for="user in users"
        :key="user.id"
        :user="user"
      />
    </div>
  </section>
</template>

<style scoped>
.users__list {
  display: grid;
  gap: 24px;
}
</style>
