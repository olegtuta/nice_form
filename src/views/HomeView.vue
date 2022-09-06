<template>
  <section class="contact-us">
    <h1 class="title">Свяжитесь с нами</h1>
    <form
        @submit.prevent="finalStep($refs.submit)"
        class="contact-form row"
    >
      <div class="form-field column x-50">
        <input
            @keyup="isEmpty($refs.name)"
            ref="name"
            class="input"
            type="text"
            required
        >
        <label class="label-for">Имя</label>
      </div>
      <div class="form-field column x-50">
        <input
            @keyup="isEmpty($refs.email)"
            ref="email"
            class="input"
            type="email"
            required
        >
        <label class="label-for">E-mail</label>
      </div>
      <div class="form-field column x-100">
        <input
            @keyup="isEmpty($refs.tel)"
            ref="tel"
            class="input"
            type="tel"
            pattern="^\+?(?:00)?\d{12}(?:#\d{2,5})?$"
            maxlength="12"
            title="Только цифры! 12 символов."
            required
        >
        <label class="label-for">Номер телефона</label>
      </div>
      <div class="form-field column x-100">
        <input
            @keyup="isEmpty($refs.message)"
            ref="message"
            class="input"
            type="text"
            required
        >
        <label class="label-for">Сообщение</label>
      </div>
      <div class="form-field column x-100 center">
        <input
            @mouseout="btnUnHover($refs.submit)"
            @mouseover="btnHover($refs.submit)"
            ref="submit"
            class="submit"
            type="submit"
            value="Отправить"
        >
      </div>
    </form>
    <div v-if="nextStep" class="next-step-container">
      <h1 class="next-step-title">Спасибо за обратную связь</h1>
      <img width="100" src="@/assets/img/good.png" alt="успешно">
    </div>
  </section>
</template>

<script setup>
import { onMounted, ref } from "vue";
import gsap from "gsap"

//Установим заголовок страницы
document.title = 'Симпатичная форма';

const nextStep = ref(false);
//Анимируем форму при загрузке страницы
onMounted(() => {

  //Постепенно будем придавать форме видимость
  gsap.to("section", {
    opacity: 1,
    duration: 4
  })

  //Тайтл будет выезжать слева к своей позиции
  gsap.from(".title", {
    x: 300,
    duration: 4
  })

  gsap.from("form", {
    x: -300,
    duration: 4
  })

})

//Плавно скругляем кнопку при наведении
const btnHover = ref => {
  gsap.to(ref, {
    borderRadius: 15,
  })
}

//Возвращаем кнопку в исходное положение
const btnUnHover = ref => {
  gsap.to(ref, {
    borderRadius: 0,
  })
}

/* Функция для установки или удаления класса
not-empty, в зависимости от пустоты поля */
const isEmpty = ref => {
  if (ref.value.length === 0) {
    ref.classList.remove("not-empty")
  } else {
    ref.classList.add("not-empty")
  }
}

//Функция для анимации формы, если успешно введены поля
const finalStep = ref => {
  gsap.to("form, .title", {
    opacity: 0
  }).then(() => {
    gsap.to("form, .title", {
      opacity: "0"
    }).then(() => {
      gsap.to("form, .title", {
        display: "none"
      }).then(() => {
        nextStep.value = true
      })
    })
  })
}
</script>

<style>
* {
  font-family: 'Lobster', cursive;
}

.center {
  text-align: center;
}

.row {
  margin: -20px 0;
}

.row:after {
  content: "";
  display: table;
  clear: both;
}

.column {
  padding: 0 20px;
  float: left;
  box-sizing: border-box;
}

.x-50 {
  width: 50%;
}

.x-100 {
  width: 100%;
}

.contact-us {
  opacity: 0;
  max-width: 650px;
  margin: 0 auto;
}

.title {
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 3px;
  font-size: 36px;
  line-height: 48px;
  padding-bottom: 48px;
  color: chocolate;
}

.next-step-title {
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 3px;
  font-size: 36px;
  line-height: 48px;
  padding-bottom: 48px;
  color: chocolate;
}

.form-field {
  position: relative;
  margin: 32px 0;
}

.input {
  display: block;
  width: 100%;
  height: 36px;
  border-width: 0 0 2px 0;
  font-size: 18px;
  line-height: 26px;
  font-weight: 400;
  border-color: lightslategray;
  background: bisque;
}

.input:focus {
  outline: none;
}

.input:focus + .label-for, .not-empty + .label-for {
  transform: translateY(-24px);
}

.label-for {
  position: absolute;
  left: 20px;
  bottom: 11px;
  font-size: 18px;
  line-height: 26px;
  font-weight: 400;
  color: #888;
  cursor: text;
  transition: transform 0.2s ease-in-out;
}

.submit {
  display: inline-block;
  background-color: chocolate;;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-size: 18px;
  line-height: 24px;
  padding: 8px 16px;
  border: none;
  cursor: pointer;
}

.next-step-container {
  text-align: center;
}

@media (max-width: 500px) {
  .title {
    font-size: 30px;
  }
}

</style>
