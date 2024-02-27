<template>
   <div class="container">
      <div class="modal">
         <div class="modal-header">
            <h3>Создание нового клиента</h3>
         </div>
         <form>
            <h2>Основные данные</h2>
            <div class="form-field">
               <BaseInput
                  :label="'Фамилия*'"
                  :id="'surname'"
                  v-model.trim="formData.surname"
                  :error="v$.surname.$error ? v$.surname.$errors : null"
               />
               <BaseInput
                  :label="'Имя*'"
                  :id="'name'"
                  v-model.trim="formData.name"
                  :error="v$.name.$error ? v$.name.$errors : null"
               />
               <BaseInput
                  :label="'Отчество*'"
                  :id="'patronymic'"
                  v-model.trim="formData.patronymic"
               />
               <BaseInput
                  :type="'date'"
                  :label="'Дата рождения*'"
                  :id="'dob'"
                  v-model.trim="formData.dob"
                  :error="v$.dob.$error ? v$.dob.$errors : null"
               />
               <BaseInput
                  :type="'tel'"
                  :label="'Номер телефона*'"
                  :id="'phone'"
                  v-model.trim="formData.phone"
                  :error="v$.phone.$error ? v$.phone.$errors : null"
               />
               <BaseInput
                  :select="true"
                  :label="'Пол*'"
                  :id="'gender'"
                  v-model.trim="formData.gender"
                  :option="genderOptions"
               />
               <BaseInput
                  :select="true"
                  :label="'Группа клиентов*'"
                  :id="'clientGroup'"
                  v-model.trim="formData.clientGroup"
                  :option="clientGroupOptions"
                  :error="v$.clientGroup.$error ? v$.clientGroup.$errors : null"
               />
               <BaseInput
                  :select="true"
                  :label="'Лечащий врач*'"
                  :id="'attendingDoctor'"
                  v-model.trim="formData.attendingDoctor"
                  :option="doctorOptions"
               />
               <BaseInput
                  :type="'checkbox'"
                  :label="'Не отправлять СМС'"
                  :id="'noSms'"
                  v-model.trim="formData.noSms"
               />
            </div>
            <h2>Адрес</h2>
            <div class="form-field">
               <BaseInput
                  :label="'Страна'"
                  :id="'country'"
                  v-model.trim="formData.address.country"
               />
               <BaseInput
                  :label="'Область'"
                  :id="'region'"
                  v-model.trim="formData.address.region"
               />
               <BaseInput
                  :label="'Город'"
                  :id="'city'"
                  v-model.trim="formData.address.city"
                  :error="
                     v$.address.city.$error ? v$.address.city.$errors : null
                  "
               />
               <BaseInput
                  :label="'Улица'"
                  :id="'street'"
                  v-model.trim="formData.address.street"
               />
               <BaseInput
                  :label="'Дом'"
                  :id="'house'"
                  v-model.trim="formData.address.house"
               />
            </div>
            <h2>Паспорт</h2>
            <div class="form-field">
               <BaseInput
                  :label="'Тип документа*'"
                  :id="'documentType'"
                  :select="true"
                  :option="documentTypesOptions"
                  v-model.trim="formData.passport.documentType"
                  :error="
                     v$.passport.documentType.$error
                        ? v$.passport.documentType.$errors
                        : null
                  "
               />
               <BaseInput
                  :label="'Серия*'"
                  :id="'series'"
                  v-model.trim="formData.passport.series"
               />
               <BaseInput
                  :label="'Номер*'"
                  :id="'number'"
                  v-model.trim="formData.passport.number"
               />
               <BaseInput
                  :label="'Кем выдан*'"
                  :id="'issuedBy'"
                  v-model.trim="formData.passport.issuedBy"
               />
               <BaseInput
                  :label="'Дата выдачи*'"
                  :id="'issueDate'"
                  :type="'date'"
                  v-model.trim="formData.passport.issueDate"
                  :error="
                     v$.passport.issueDate.$error
                        ? v$.passport.issueDate.$errors
                        : null
                  "
               />
            </div>
         </form>
         <div class="modal-footer">
            <button @click.prevent="submitForm">Создать Клиента</button>
         </div>
      </div>
   </div>
</template>

<script>
import { ref } from 'vue'
import { required, numeric, minLength } from '@vuelidate/validators'
import BaseInput from './components/BaseInput.vue'
import useVuelidate from '@vuelidate/core'

export default {
   components: {
      BaseInput,
   },
   setup() {
      // Статические данные
      const formData = ref({
         surname: '',
         name: '',
         patronymic: '',
         dob: '',
         phone: '',
         gender: '',
         clientGroup: [],
         attendingDoctor: '',
         noSms: false,
         address: {
            index: '',
            country: '',
            region: '',
            city: '',
            street: '',
            house: '',
         },
         passport: {
            documentType: '',
            series: '',
            number: '',
            issuedBy: '',
            issueDate: '',
         },
      })

      const genderOptions = [
         { value: 'male', title: 'Мужской' },
         { value: 'female', title: 'Женский' },
      ]

      const clientGroupOptions = [
         { value: 'VIP', title: 'VIP' },
         { value: 'Проблемные', title: 'Проблемные' },
         { value: 'ОМС', title: 'ОМС' },
      ]

      const doctorOptions = [
         { value: 'Иванов', title: 'Иванов' },
         { value: 'Захаров', title: 'Захаров' },
         { value: 'Чернышева', title: 'Чернышева' },
      ]

      const documentTypesOptions = [
         { value: 'Паспорт', title: 'Паспорт' },
         {
            value: 'Свидетельство о рождении',
            title: 'Свидетельство о рождении',
         },
         { value: 'Вод. удостоверение', title: 'Вод. удостоверение' },
      ]

      // Валидации
      const validations = {
         surname: { required },
         name: { required },
         dob: { required },
         phone: { required, numeric, minLength: minLength(11) },
         clientGroup: { required },
         address: {
            city: { required },
         },
         passport: {
            documentType: { required },
            issueDate: { required },
         },
      }

      // Валидация формы

      const v$ = useVuelidate(validations, formData)

      // Метод отправки формы
      const submitForm = async () => {
         const result = await v$.value.$validate()

         if (result) {
            console.log('pass')
            console.log(formData)
         } else {
            console.log(v$.value.$error)
         }
      }

      // Возвращаемые значения
      return {
         formData,
         genderOptions,
         clientGroupOptions,
         documentTypesOptions,
         doctorOptions,
         validations,
         v$,
         submitForm,
      }
   },
}
</script>

<style lang="scss" scoped>
.container {
   width: 100%;
   height: 100vh;
   background: #f4f4f4;
   margin: 0 auto;
   padding: 10px;
   display: flex;
   align-items: center;
   justify-content: center;
}

.modal {
   box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
   border-radius: 5px;
   width: 100%;
   max-width: 1200px;
   height: 600px;
   display: flex;
   flex-direction: column;
   background: white;
}

.modal > * {
   padding: 15px 20px;
}

.modal > *:not(:last-child) {
   border-bottom: 1px solid lightgray;
}

::-webkit-scrollbar {
   width: 8px;
   border-radius: 20px;
}

/* Track */
::-webkit-scrollbar-track {
   background: transparent;
}

/* Handle */
::-webkit-scrollbar-thumb {
   background: #a9a9a9;
   border-radius: 20px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
   background: #555;
}

form {
   flex: 1;
   overflow: scroll;
   overflow-x: hidden;
}
form h2 {
   margin-bottom: 20px;
   font-weight: bold;
}

.form-field {
   display: grid;
   grid-template-columns: repeat(auto-fill, minmax(360px, 1fr));
   gap: 20px;
}

.form-field:not(:last-child) {
   margin-bottom: 40px;
}

button {
   padding: 0.5rem 1rem;
   font-size: 1rem;
   background-color: #007bff;
   color: #fff;
   border: none;
   border-radius: 4px;
   cursor: pointer;
}

.checkbox-container {
   display: flex;
   align-items: center;
}

.checkbox-container input[type='checkbox'] {
   margin-right: 0.5rem;
}

@media (max-width: 765px) {
   .modal {
      height: 100%;
   }

   .form-field {
      grid-template-columns: 1fr;
   }

   .modal > * {
      padding: 15px 10px;
   }

   button {
      width: 100%;
   }
}
</style>
