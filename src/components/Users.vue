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
