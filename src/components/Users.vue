<script setup lang="ts">
import {user} from '@/data/users'

const fullName = `${user.name.first} ${user.name.last}`
const titledName = `${user.name.title} ${fullName}`
const genderLabel = user.gender === 'female' ? 'Жінка' : 'Чоловік'
const birthDate = new Date(user.dob.date).toLocaleDateString('uk-UA', {timeZone: 'UTC'})
const shortAddress = `${user.location.city}, ${user.location.state}, ${user.location.country}`
const street = `${user.location.street.number} ${user.location.street.name}`
const timezone = `${user.location.timezone.offset} (${user.location.timezone.description})`
</script>

<template>
  <article class="user-card">
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
        <span>{{ user.dob.age }} р.</span>
      </p>

      <ul class="user-card__contacts">
        <li>📍 {{ shortAddress }}</li>
        <li>✉️ {{ user.email }}</li>
        <li>📞 {{ user.phone }}</li>
        <li>📱 {{ user.cell }}</li>
      </ul>
    </section>

    <section class="user-card__main">
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
