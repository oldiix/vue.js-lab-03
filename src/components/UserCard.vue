<script setup lang="ts">
import {computed, ref} from 'vue'
import type {AgeGroup, User} from '@/types/user'

const props = defineProps<{
  user: User
}>()

const isDetailsVisible = ref(false)

const fullName = computed(() => `${props.user.name.first} ${props.user.name.last}`)
const titledName = computed(() => `${props.user.name.title} ${fullName.value}`)
const age = computed(() => props.user.dob.age)
const isAdult = computed(() => age.value >= 18)

const ageGroup = computed<AgeGroup>(() => {
  if (age.value < 18) return 'minor'
  if (age.value <= 30) return 'young'
  if (age.value <= 50) return 'adult'
  return 'senior'
})

const genderLabel = computed(() => (props.user.gender === 'female' ? 'Жінка' : 'Чоловік'))

const birthDate = computed(() =>
  new Date(props.user.dob.date).toLocaleDateString('uk-UA', {timeZone: 'UTC'}),
)

const shortAddress = computed(() => {
  const {city, state, country} = props.user.location
  return `${city}, ${state}, ${country}`
})

const street = computed(() => {
  const {number, name} = props.user.location.street
  return `${number} ${name}`
})

const timezone = computed(() => {
  const {offset, description} = props.user.location.timezone
  return `${offset} (${description})`
})

function toggleDetails(): void {
  isDetailsVisible.value = !isDetailsVisible.value
}
</script>

<template>
  <article
    class="user-card"
    :class="{
      'user-card--minor': ageGroup === 'minor',
      'user-card--young': ageGroup === 'young',
      'user-card--adult': ageGroup === 'adult',
      'user-card--senior': ageGroup === 'senior',
    }"
  >
    <section class="user-card__aside">
      <img
        class="user-card__photo"
        :src="user.picture"
        :alt="`Фото: ${fullName}`"
        width="320"
        height="280"
      >

      <h2 class="user-card__name">
        {{ titledName }}
      </h2>

      <p class="user-card__meta">
        <span>{{ genderLabel }}</span>
        <span class="user-card__divider" />
        <span v-if="isAdult">{{ age }} р.</span>
        <span
          v-else
          class="user-card__muted"
        >вік приховано</span>
      </p>

      <span class="user-card__badge">{{ ageGroup }}</span>

      <ul class="user-card__contacts">
        <li>📍 {{ shortAddress }}</li>
        <li>✉️ {{ user.email }}</li>
        <li>📞 {{ user.phone }}</li>
        <li>📱 {{ user.cell }}</li>
      </ul>
    </section>

    <section class="user-card__main">
      <button
        class="user-card__toggle"
        type="button"
        :aria-expanded="isDetailsVisible"
        @click="toggleDetails"
      >
        <span>Про мене</span>
        <span>{{ isDetailsVisible ? '▲' : '▼' }}</span>
      </button>
      <p
        v-show="isDetailsVisible"
        class="user-card__details"
      >
        {{ user.details }}
      </p>

      <h3 class="user-card__heading">
        Особиста інформація
      </h3>
      <dl class="user-card__info">
        <dt>Повне ім'я</dt>
        <dd>{{ titledName }}</dd>
        <dt>Стать</dt>
        <dd>{{ genderLabel }}</dd>
        <dt>Дата народження</dt>
        <dd>{{ birthDate }}</dd>
        <dt>Email</dt>
        <dd>{{ user.email }}</dd>
        <dt>Телефон</dt>
        <dd>{{ user.phone }}</dd>
        <dt>Мобільний</dt>
        <dd>{{ user.cell }}</dd>
      </dl>

      <h3 class="user-card__heading">
        Адреса
      </h3>
      <dl class="user-card__info">
        <dt>Вулиця</dt>
        <dd>{{ street }}</dd>
        <dt>Місто</dt>
        <dd>{{ user.location.city }}</dd>
        <dt>Регіон</dt>
        <dd>{{ user.location.state }}</dd>
        <dt>Країна</dt>
        <dd>{{ user.location.country }}</dd>
        <dt>Індекс</dt>
        <dd>{{ user.location.postcode }}</dd>
        <dt>Часовий пояс</dt>
        <dd>{{ timezone }}</dd>
      </dl>

      <h3 class="user-card__heading">
        Хобі
      </h3>
      <ul class="user-card__hobbies">
        <li
          v-for="hobby in user.hobbies"
          :key="hobby"
          class="user-card__hobby"
        >
          {{ hobby }}
        </li>
      </ul>
    </section>
  </article>
</template>

<style scoped>
.user-card {
  --accent: #64748b;
  --accent-soft: #f1f5f9;

  display: grid;
  grid-template-columns: minmax(240px, 2fr) 3fr;
  overflow: hidden;
  border: 1px solid #e2e8f0;
  border-top: 6px solid var(--accent);
  border-radius: 16px;
  background: #fff;
  box-shadow: 0 8px 24px rgb(15 23 42 / 8%);
}

.user-card--minor {
  --accent: #0d9488;
  --accent-soft: #ccfbf1;
}

.user-card--young {
  --accent: #2563eb;
  --accent-soft: #dbeafe;
}

.user-card--adult {
  --accent: #7c3aed;
  --accent-soft: #ede9fe;
}

.user-card--senior {
  --accent: #d97706;
  --accent-soft: #fef3c7;
}

.user-card__aside {
  padding: 24px;
  border-right: 1px solid #e2e8f0;
}

.user-card__photo {
  display: block;
  width: 100%;
  max-width: 320px;
  height: auto;
  aspect-ratio: 8 / 7;
  object-fit: cover;
  border-radius: 12px;
  background: var(--accent-soft);
}

.user-card__name {
  margin: 20px 0 8px;
  font-size: 1.75rem;
  color: #0f172a;
}

.user-card__meta {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: 0 0 12px;
}

.user-card__divider {
  width: 1px;
  height: 20px;
  background: #cbd5e1;
}

.user-card__muted {
  color: #94a3b8;
  font-style: italic;
}

.user-card__badge {
  display: inline-block;
  padding: 2px 12px;
  border-radius: 999px;
  background: var(--accent-soft);
  color: var(--accent);
  font-size: 0.8rem;
  font-weight: 600;
  text-transform: uppercase;
}

.user-card__contacts {
  display: grid;
  gap: 10px;
  margin: 20px 0 0;
  padding: 0;
  list-style: none;
  overflow-wrap: anywhere;
}

.user-card__main {
  padding: 24px;
}

.user-card__toggle {
  display: flex;
  justify-content: space-between;
  width: 100%;
  padding: 14px 18px;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  background: var(--accent-soft);
  color: #0f172a;
  font: inherit;
  font-weight: 600;
  cursor: pointer;
}

.user-card__details {
  margin: 8px 0 0;
  padding: 12px 18px;
  border-left: 4px solid var(--accent);
  background: #f8fafc;
}

.user-card__heading {
  margin: 24px 0 12px;
  color: #0f172a;
}

.user-card__info {
  display: grid;
  grid-template-columns: 160px 1fr;
  gap: 8px 16px;
  margin: 0;
}

.user-card__info dt {
  color: #64748b;
}

.user-card__info dd {
  margin: 0;
  color: #0f172a;
  overflow-wrap: anywhere;
}

.user-card__hobbies {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin: 0;
  padding: 0;
  list-style: none;
}

.user-card__hobby {
  padding: 6px 14px;
  border-radius: 999px;
  background: var(--accent-soft);
  color: var(--accent);
  font-size: 0.9rem;
}

@media (max-width: 760px) {
  .user-card {
    grid-template-columns: 1fr;
  }

  .user-card__aside {
    border-right: none;
    border-bottom: 1px solid #e2e8f0;
  }

  .user-card__info {
    grid-template-columns: 1fr;
    gap: 2px;
  }

  .user-card__info dd {
    margin-bottom: 8px;
  }
}
</style>
