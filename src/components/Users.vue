<script setup lang="ts">
import {computed, ref} from 'vue'
import UserCard from '@/components/UserCard.vue'
import {users as usersData} from '@/data/users'
import type {AgeFilter, GenderFilter, SortOption, User} from '@/types/user'

interface ToolbarOption<T> {
  value: T
  label: string
}

const genderOptions: ToolbarOption<GenderFilter>[] = [
  {value: 'all', label: 'Всі'},
  {value: 'male', label: 'Чоловіки'},
  {value: 'female', label: 'Жінки'},
]

const ageOptions: ToolbarOption<AgeFilter>[] = [
  {value: 'all', label: 'Всі'},
  {value: 'adult', label: '18+'},
]

const sortOptions: ToolbarOption<SortOption>[] = [
  {value: 'name-asc', label: "Ім'я ↑"},
  {value: 'name-desc', label: "Ім'я ↓"},
  {value: 'age-asc', label: 'Вік ↑'},
  {value: 'age-desc', label: 'Вік ↓'},
]

const users = ref<User[]>(usersData)

const genderFilter = ref<GenderFilter>('all')
const ageFilter = ref<AgeFilter>('all')
const sortOption = ref<SortOption>('none')

const filteredUsers = computed(() =>
  users.value.filter((user) => {
    const matchesGender = genderFilter.value === 'all' || user.gender === genderFilter.value
    const matchesAge = ageFilter.value === 'all' || user.dob.age >= 18
    return matchesGender && matchesAge
  }),
)

const visibleUsers = computed(() => {
  const result = [...filteredUsers.value]

  switch (sortOption.value) {
    case 'name-asc':
      return result.sort(compareByName)
    case 'name-desc':
      return result.sort((a, b) => compareByName(b, a))
    case 'age-asc':
      return result.sort((a, b) => a.dob.age - b.dob.age)
    case 'age-desc':
      return result.sort((a, b) => b.dob.age - a.dob.age)
    default:
      return result
  }
})

function compareByName(a: User, b: User): number {
  return `${a.name.first} ${a.name.last}`.localeCompare(`${b.name.first} ${b.name.last}`)
}
</script>

<template>
  <section class="users">
    <div class="users__toolbar">
      <div class="users__group">
        <span class="users__label">Стать:</span>
        <button
          v-for="option in genderOptions"
          :key="option.value"
          class="users__button"
          :class="{ 'users__button--active': genderFilter === option.value }"
          type="button"
          @click="genderFilter = option.value"
        >
          {{ option.label }}
        </button>
      </div>

      <div class="users__group">
        <span class="users__label">Вік:</span>
        <button
          v-for="option in ageOptions"
          :key="option.value"
          class="users__button"
          :class="{ 'users__button--active': ageFilter === option.value }"
          type="button"
          @click="ageFilter = option.value"
        >
          {{ option.label }}
        </button>
      </div>

      <div class="users__group">
        <span class="users__label">Сортування:</span>
        <button
          v-for="option in sortOptions"
          :key="option.value"
          class="users__button"
          :class="{ 'users__button--active': sortOption === option.value }"
          type="button"
          @click="sortOption = option.value"
        >
          {{ option.label }}
        </button>
      </div>
    </div>

    <p class="users__counter">
      Показано {{ visibleUsers.length }} з {{ users.length }}
    </p>

    <p
      v-if="users.length === 0"
      class="users__empty"
    >
      Список юзерів пустий
    </p>
    <p
      v-else-if="visibleUsers.length === 0"
      class="users__empty"
    >
      Немає юзерів, що відповідають фільтрам
    </p>
    <div
      v-else
      class="users__list"
    >
      <UserCard
        v-for="user in visibleUsers"
        :key="user.id"
        :user="user"
      />
    </div>
  </section>
</template>

<style scoped>
.users__toolbar {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 12px 24px;
  margin-bottom: 12px;
  padding: 16px;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  background: #fff;
}

.users__group {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 6px;
}

.users__label {
  color: #64748b;
  font-weight: 600;
}

.users__button {
  padding: 6px 14px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  background: #fff;
  color: #0f172a;
  font: inherit;
  cursor: pointer;
}

.users__button:hover:not(:disabled) {
  border-color: #2563eb;
}

.users__button--active {
  border-color: #2563eb;
  background: #2563eb;
  color: #fff;
}

.users__counter {
  margin: 0 0 16px;
  color: #64748b;
}

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
