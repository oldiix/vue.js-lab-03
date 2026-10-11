<script setup lang="ts">
import {ref} from 'vue'
import UserCard from '@/components/UserCard.vue'
import {users as usersData} from '@/data/users'
import type {User} from '@/types/user'

const users = ref<User[]>(usersData)
</script>

<template>
  <section class="users">
    <p
      v-if="users.length === 0"
      class="users__empty"
    >
      Список юзерів пустий
    </p>
    <div
      v-else
      class="users__list"
    >
      <UserCard
        v-for="user in users"
        :key="user.id"
        :user="user"
      />
    </div>
  </section>
</template>

<style scoped>
.users__empty {
  padding: 40px;
  border: 2px dashed #cbd5e1;
  border-radius: 12px;
  color: #64748b;
  text-align: center;
}

.users__list {
  display: grid;
  gap: 24px;
}
</style>
