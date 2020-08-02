<template>
  <div>
    <form novalidate>
      <fieldset v-show="nextSlideCount === 1" id="commonInfo">
        <legend>Основная информация</legend>

        <div class="inputField">
          <label for="surname">*Фамилия:</label>
          <input
            v-bind:class="{error: $v.commonInfo.surname.$error}"
            type="text"
            id="surname"
            v-model.trim="$v.commonInfo.surname.$model"
          />
          <span
            v-if="$v.commonInfo.surname.$dirty && !$v.commonInfo.surname.required"
          >Это поле обязательно для заполнения</span>
          <span v-if="!$v.commonInfo.surname.alphabet_cyrillic">В этом поле не должно быть цифр</span>
        </div>

        <div class="inputField">
          <label for="name">*Имя:</label>
          <input
            v-bind:class="{error: $v.commonInfo.name.$error}"
            type="text"
            id="name"
            v-model="commonInfo.name"
          />
          <span
            v-if="$v.commonInfo.name.$dirty && !$v.commonInfo.name.required"
          >Это поле обязательно для заполнения</span>
          <span v-if="!$v.commonInfo.name.alphabet_cyrillic">В этом поле не должно быть цифр</span>
        </div>

        <div class="inputField">
          <label for="patronymic">Отчество:</label>
          <input type="text" id="patronymic" />
        </div>
        <div class="inputField">
          <label for="dateOfBirth">*Дата рождения:</label>
          <input
            v-bind:class="{error: $v.commonInfo.dateOfBirth.$error}"
            type="text"
            id="dateOfBirth"
            v-model.trim="$v.commonInfo.dateOfBirth.$model"
          />
          <span v-if="$v.commonInfo.dateOfBirth.$error">Дата должна быть в формате ДД.ММ.ГГГГ</span>
        </div>

        <div class="inputField">
          <label for="phone">*Телефон:</label>
          <input
            v-bind:class="{error: $v.commonInfo.phone.$error}"
            type="phone"
            id="phone"
            v-model.trim="$v.commonInfo.phone.$model"
          />
          <span
            v-if="$v.commonInfo.phone.$dirty && !$v.commonInfo.phone.required"
          >Это поле обязательно для заполнения</span>
          <span v-if="!$v.commonInfo.phone.isPhoneValide">Должен содержать 11 цифр и начинаться с +7</span>
        </div>

        <div class="inputField">
          <label for="sex">Пол:</label>
          <div id="sex">
            <label for="male">Муж</label>
            <input type="radio" id="male" name="gender" value="male" />

            <label for="female">Жен</label>
            <input type="radio" id="female" name="gender" value="female" />

            <label for="other">Другой</label>
            <input type="radio" id="other" name="gender" value="other" />
          </div>
        </div>
        <div class="inputField">
          <label for="clientGroup">*Группа клиентов:</label>
          <select
            v-bind:class="{error: $v.commonInfo.clientGroup.$error}"
            multiple
            id="clientGroup"
            v-model="$v.commonInfo.clientGroup.$model"
          >
            <option value="VIP">VIP</option>
            <option value="Проблемные">Проблемные</option>
            <option value="ОМС">ОМС</option>
          </select>
          <span
            v-if="$v.commonInfo.clientGroup.$dirty && !$v.commonInfo.clientGroup.required"
          >Это поле обязательно для заполнения</span>
        </div>

        <div class="inputField">
          <label for="attendingDoctor">Лечащий врач:</label>
          <select id="attendingDoctor">
            <option v-for="item in commonInfo.attdoctor" v-bind:key="item.doctor">{{item.doctor}}</option>
          </select>
        </div>
        <div class="inputField">
          <label for="noSendSMS">Не отправлять СМС:</label>
          <input type="checkbox" id="noSendSMS" />
        </div>
        <div class="buttonGroup">
          <button
            v-on:click.prevent="checkValide($v.commonInfo.$invalid)"
            class="button"
            type="button"
          >Далее</button>
        </div>
      </fieldset>

      <fieldset v-show="nextSlideCount === 2" id="adress">
        <legend>Адресс</legend>

        <div class="inputField">
          <label for="index">Индекс:</label>
          <input type="text" id="index" />
        </div>

        <div class="inputField">
          <label for="country">Страна:</label>
          <input type="text" id="country" />
        </div>

        <div class="inputField">
          <label for="region">Область:</label>
          <input type="text" id="region" />
        </div>

        <div class="inputField">
          <label for="city">*Город:</label>
          <input
            v-bind:class="{error: $v.adress.city.$error}"
            type="text"
            id="city"
            v-model.trim="$v.adress.city.$model"
          />
          <span
            v-if="$v.adress.city.$dirty && !$v.adress.city.required"
          >Это поле обязательно для заполнения</span>
          <span v-if="!$v.adress.city.alphabet_cyrillic">В этом поле не должно быть цифр</span>
        </div>

        <div class="inputField">
          <label for="street">Улица:</label>
          <input type="text" id="street" />
        </div>

        <div class="inputField">
          <label for="houseNumber">Дом:</label>
          <input type="number" id="houseNumber" />
        </div>
        <div class="buttonGroup">
          <button v-on:click="previousSlide">Назад</button>

          <button class="button" type="button" v-on:click="checkValide($v.adress.$invalid)">Далее</button>
        </div>
      </fieldset>

      <fieldset v-show="nextSlideCount === 3" id="passport">
        <legend>Пасспорт</legend>

        <div class="inputField">
          <label for="docType">*Тип документа:</label>
          <select
            v-bind:class="{error: $v.passport.docType.$error}"
            id="docType"
            v-model.trim="$v.passport.docType.$model"
          >
            <option value="Пасспорт">Пасспорт</option>
            <option value="Свидетльство о рождении">Свидетельство о рождении</option>
            <option value="Водительское удостоверение">Вод. удостоверение</option>
          </select>
          <span
            v-if="$v.passport.docType.$dirty && !$v.passport.docType.required"
          >Это поле обязательно для заполнения</span>
        </div>

        <div class="inputField">
          <label for="seriesNumb">Серия:</label>
          <input type="number" id="seriesNumb" />
        </div>

        <div class="inputField">
          <label for="docNumb">Номер:</label>
          <input type="number" id="docNumb" />
        </div>

        <div class="inputField">
          <label for="issuedBy">Кем выдан:</label>
          <input type="text" id="issuedBy" />
        </div>
        <div class="inputField">
          <label for="dateBy">*Дата выдачи:</label>
          <input
            v-bind:class="{error: $v.passport.dateBy.$error}"
            type="date"
            id="dateBy"
            v-model.trim="$v.passport.dateBy.$model"
          />
          <span
            v-if="$v.passport.dateBy.$dirty && !$v.passport.dateBy.required"
          >Это поле обязательно для заполнения</span>
        </div>
        <div class="buttonGroup">
          <button type="button" class="button" v-on:click="previousSlide">Назад</button>
          <button
            class="button"
            type="button"
            v-on:click="checkValide($v.passport.$invalid), displayVisibility=true"
          >Создать клиента</button>
        </div>
      </fieldset>
    </form>

    <div class="congrat" v-if="nextSlideCount===4">
      <p>Поздравляем, вы успешно создали клиента!</p>
      <img src="@/assets/images/smailik1.png" alt="sdf" />
    </div>
  </div>
</template>

<script>
import { required, helpers } from "vuelidate/lib/validators";

const alphabet_cyrillic = helpers.regex(
  "alpha",
  /^([а-яА-ЯёЁ\s]|[a-zA-Z\s])*$/
);

const isPhoneValide = helpers.regex("+79009019129", /^\+7(\d{10})$/);

const isDateValide = helpers.regex(
  "12.05.1995",
  /(0?[1-9]|[12][0-9]|3[01]).(0?[1-9]|1[0-2]).((19|20)\d\d)/
);

export default {
  data() {
    return {
      commonInfo: {
        name: "",
        surname: null,
        dateOfBirth: null,
        phone: null,
        attdoctor: [
          { doctor: "Иванов" },
          { doctor: "Захаров" },
          { doctor: "Чернышева" },
        ],
        clientGroup: ["VIP", "Проблемные", "ОМС"],
      },

      adress: {
        city: null,
      },

      passport: {
        docType: null,
        dateBy: null,
      },

      nextSlideCount: 1,
      nextSlideStatus: null,
      displayVisibility: false,
    };
  },

  validations: {
    commonInfo: {
      surname: {
        required,
        alphabet_cyrillic,
      },
      name: {
        required,
        alphabet_cyrillic,
      },
      phone: {
        required,
        isPhoneValide,
      },
      dateOfBirth: {
        required,
        isDateValide,
      },
      clientGroup: {
        required,
      },
    },
    adress: {
      city: {
        required,
        alphabet_cyrillic,
      },
    },
    passport: {
      docType: {
        required,
        alphabet_cyrillic,
      },
      dateBy: {
        required,
      },
    },
  },

  // __________________________________________________________________
  //1futile attempt to link data to localstorage and update them when slide switched back
  // mounted() {
  //   if (localStorage.name) {
  //     this.commonInfo.name = localStorage.name;
  //     this.commonInfo.dateOfBirth = localStorage.dateOfBirth;
  //   }
  // },
  // ___________________________________________________________________
  //nextSlide switches slides; slides means fieldset

  methods: {
    nextSlide() {
      if (this.nextSlideStatus === "OK") {
        this.persist();
      }
    },
    //previous Slide switches back slides
    previousSlide() {
      this.nextSlideCount--;
    },
    // ________________________________________________________________
    //2futile attempt to link data to localstorage and update them when slide switched back
    // persist() {
    //   localStorage.name = this.commonInfo.name;
    //   localStorage.dateOfBirth = this.commonInfo.dateOfBirth;
    // },
    // ____________________________________________________________________

    //checkValide checks the slide for validity
    //and switches the slide if conditions match
    checkValide(status) {
      if (this.nextSlideCount === 1) {
        this.$v.commonInfo.$touch();
      }
      if (this.nextSlideCount === 2) {
        this.$v.adress.$touch();
      }
      if (this.nextSlideCount === 3) {
        this.$v.passport.$touch();
      }

      if (status) {
        this.nextSlideStatus = "ERROR";
      } else {
        this.nextSlideStatus = "PENDING";

        setTimeout(() => {
          this.nextSlideCount++;

          this.nextSlideStatus = "OK";
        }, 500);
      }
    },
  },
  // ________________________________________________________________
  //3futile attempt to link data to localstorage and update them when slide switched back
  // watch: {
  //   name(newName) {
  //     localStorage.name = newName;
  //   },
  // deep= true
  // },
  // ________________________________________________________________
};
</script>

<style lang ="sass">
</style>
