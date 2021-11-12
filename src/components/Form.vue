<template>
  <div class="container">
    <form
      autocomplete="off"
      class="form"
      @submit.prevent="formSubmit" 
    >
      <h2>Личные данные</h2>
      <div class="row col-3">
        <div>
          <label for="lastName">Фамилия</label>
          <input 
            class="input-text"
            v-model="formData.general.lastName" 
            id="lastName" required />
        </div>
        <div>
          <label for="firstName">Имя</label>
          <input 
            class="input-text"
            v-model="formData.general.firstName" 
            id="firstName" required />
        </div>
        <div>
          <label for="patronymic">Отчество</label>
          <input 
            class="input-text"
            v-model="formData.general.patronymic" 
            id="patronymic" />
        </div>
      </div>
      <div class="row">
        <label for="dateBirth">Дата рождения</label>
        <input
          class="input-text"
          v-model="formData.general.dateBirth"
          id="dateBirth"
          placeholder="дд.мм.гггг"
        >
      </div>
      <div class="row">
        <label for="email">Email</label>
        <input
          class="input-text"
          v-model="formData.general.email"
          id="email"
          placeholder="example@mail.com"
          required
        >
      </div>
      <h2>Пол</h2>
      <div class="row radio">
        <input v-model="formData.general.gender" name="gender" type="radio" id="radioMale" value="male">
        <label for="radioMale">Мужской</label>
        <input v-model="formData.general.gender" name="gender" type="radio" id="radioFemale" value="female">
        <label for="radioFemale">Женский</label>
      </div>
      <h2>Паспортные данные</h2>
      <div class="row citizenship">
        <div class="citizenship-selector"
        > 
          <label for="citizenship">Гражданство</label>
          <input
            class="input-text"
            id="citizenship"
            v-model="formData.general.nationality"
            @focus="isDropdownOpen = true"
             v-click-outside="hideDropdown"
             required
          />
          <div
            v-if="isDropdownOpen"
            class="citizenship-selector__dropdown"
          >
            <ul v-if="allCitizenships.length">
              <li
                v-for="item in allCitizenships"
                :key="item._id"
                @click="onItemClick(item)"
              >
                {{ item.nationality }}
              </li>
            </ul>
            <div
              v-else
              class="empty"
            >
              Ничего не найдено
            </div>
          </div>
        </div>
      </div>
      <div class="row passport col-3"
        v-if="formData.general.nationality === 'Russia'"
      >
        <div>
          <label for="passport-rus-series">Серия паспорта</label>
          <input
            class="input-text"
            v-model="formData.passportRus.series"
            id="passport-rus-series"
            required
          >
        </div>
        <div>
          <label for="passport-rus-number">Номер паспорта</label>
          <input
            class="input-text"
            v-model="formData.passportRus.number"
            id="passport-rus-number"
            required
          >
        </div>
        <div>
          <label for="passport-rus-date">Дата выдачи</label>
          <input
            class="input-text"
            v-model="formData.passportRus.date"
            id="passport-rus-date"
            placeholder="дд.мм.гггг"
            required
          >
        </div>
      </div>
      <div class="row col-3"
        v-if="formData.general.nationality !== 'Russia' && formData.general.nationality.length"
      >
        <div>
          <label for="passport-number">Номер паспорта</label>
          <input
            class="input-text"
            v-model="formData.passport.number"
            id="passport-number"
            required
          >
        </div>
        <div 
          class="passport-country-selector"
        >
          <label for="passport-country">Страна выдачи</label>
          <input
            class="input-text"
            v-model="formData.passport.country"
            id="passport-country"
            @focus="isCountryPassportOpen = true"
            v-click-outside="hideCountryDropdown"
            required
          />
          <div
            v-if="isCountryPassportOpen"
            class="passport-country__dropdown"
          >
            <ul v-if="allCitizenships.length">
              <li
                v-for="item in allCitizenships"
                :key="item._id"
                @click="onPassportCountryClick(item)"
              >
                {{ item.nationality }}
              </li>
            </ul>
            <div
              v-else
              class="empty"
            >
              Ничего не найдено
            </div>
          </div>
        </div>
        <div>
          <label for="passport-type">Тип паспорта</label>
          <input
            class="input-text"
            v-model="formData.passport.type"
            id="passport-type"
            @focus="isDropdownPassType = true"
            v-click-outside="hidePassDropdown"
            required
          >
          <div
            v-if="isDropdownPassType"
            class="passport-type-selector__dropdown"
          >
            <ul v-if="passportTypes.length">
              <li
                v-for="item in passportTypes"
                :key="item.id"
                @click="onPassportTypeClick(item)"
              >
                {{ item.type }}
              </li>
            </ul>
            <div
              v-else
              class="empty"
            >
              Ничего не найдено
            </div>
          </div>
        </div>
      </div>
      <h2>Меняли ли фамилию или имя?</h2>
      <div class="row radio">
        <input 
          v-model="formData.general.changeSurname" 
          name="change-surname" 
          type="radio" 
          id="radio-no" 
          value="no">
        <label for="radio-no">Нет</label>
        <input 
          v-model="formData.general.changeSurname" 
          name="change-surname" 
          type="radio" 
          id="radio-yes" 
          value="yes">
        <label for="radio-yes">Да</label>
      </div>

      <div class="row col-2" v-if="formData.general.changeSurname === 'yes'">
        <div>
          <label for="surname-new">Фамилия</label>
          <input
            class="input-text"
            id="surname-new"
            v-model="formData.general.newSurname"
              required
          />
        </div>
        <div>
          <label for="name-new">Имя</label>
          <input
            class="input-text"
            id="name-new"
            v-model="formData.general.newName"
              required
          />
        </div>
      </div>

      <div class="row">
        <button class="button-submit">Отправить</button>
      </div>

    </form>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside"
import citizenships from "../assets/data/citizenships.json"
import passTypes from "../assets/data/passport-types.json"

export default {
  directives: {
    ClickOutside,
  },
  data() {
    return {
      formData: {
        general: {
          patronymic: '',
          lastName: '',
          firstName: '',
          dateBirth: '',
          email: '',
          nationality: '',
          gender: 'male',
          changeSurname: 'no',
          newSurname: '',
          newName: ''
        },
        passportRus: {
          series: '',
          number: '',
          date: ''
        },
        passport: {
          series: '',
          country: '',
          type: ''
        },
      },
     
      isDropdownOpen: false,
      isCountryPassportOpen: false,
      isDropdownPassType: false,
      allCitizenships: citizenships,
      passportTypes: passTypes
    };
  },
  methods: {
    hideDropdown () {
      this.isDropdownOpen = false
    },
    hideCountryDropdown () {
      this.isCountryPassportOpen = false
    },
    hidePassDropdown () {
      this.isDropdownPassType = false
    },
    onItemClick (item) {
      this.formData.general.nationality = item.nationality
      this.isDropdownOpen = false
    },
    onPassportCountryClick (item) {
      this.formData.passport.country = item.nationality
      this.isCountryPassportOpen = false
    },
    onPassportTypeClick (item) {
      this.formData.passport.type = item.type
      this.isDropdownPassType = false
    },
    formSubmit () {
      console.log(JSON.stringify(this.formData))
    }
  },
};
</script>

<style lang='scss' scoped>
  
  @mixin dropdown {
    position: absolute;
    height: 100px;
    width: 300px;
    background: white;
    overflow-y: auto;
    border-radius: 5px;
    box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22);
    ul {
      list-style: none;
      margin-top: 5px;
      margin-bottom: 5px;
      padding-left: 3px;
      & > li {
        padding-right: 0 10px;
      }
      & > li:hover {
        cursor: pointer;
        background: rgb(224, 232, 251);
      }
    }
  }
  body {
    margin: 0;
  }
  .container {
    width: 100%;
    max-width: 1200px;
    padding: 30px;
    margin: 0 auto;
  }
  input, label {
    display:block;
  }
  label {
    margin-bottom: 10px;
  }
  input {
    padding: 0 10px;
  }
  .row {
    padding: 20px;
    width: 100%;
    input {
      height: 40px;
    }
    .input-text {
      width: 300px;
    }
    &.radio {
      display: flex;
      input {
        height: unset;
      }
      label {
        margin-right: 20px;
      }
    }
  }
  .col-3 {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    & > div {
      flex: 0 1 calc(33.333% - 10px);
      margin-right: 10px;
    }
    & > div:last-of-type {
      margin-right: 0px;
    }
  }
  .col-2 {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    & > div {
      flex: 0 1 calc(50% - 20px);
      margin-right: 20px;
    }
    & > div:last-of-type {
      margin-right: 0px;
    }
  }
  .citizenship-selector {
    position: relative;
    input {
      width: 200px;
    }
    &__dropdown {
      @include dropdown;
    }
  }
  .passport-country {
    &__dropdown {
      @include dropdown;
    }
  }
  .passport-type-selector {
    &__dropdown {
      @include dropdown;
    }
  }
  .button-submit {
    color: white;
    font-size: 20px;
    background-color: cornflowerblue;
    border: none;
    height: 50px;
    width: 200px;
    border-radius: 5px;
    transition: ease-in-out .2s;
    &:hover {
      cursor: pointer;
      background-color: rgb(90, 131, 207);
    }
  }
</style>
