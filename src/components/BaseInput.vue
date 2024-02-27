<template>
   <div>
      <label :for="id">
         {{ label }}
      </label>
      <select v-if="select" :id="id" @input="inputHandler">
         <option v-for="item in option" :key="item.value" :value="item.value">
            {{ item.title }}
         </option>
      </select>

      <input
         v-else-if="id !== 'phone'"
         :type="type"
         :id="id"
         @input="inputHandler"
         autocomplete="off"
      />

      <div class="phone-field" v-else>
         <input class="phone-field__prefix" type="text" value="+7" />
         <input
            :type="type"
            :id="id"
            @input="inputPhoneHandler"
            autocomplete="off"
            :maxlength="id === 'phone' ? 10 : false"
         />
      </div>

      <span v-if="props.error">
         {{ getErrorMessage(props.error[0].$message) }}
      </span>
   </div>
</template>

<script setup>
const props = defineProps({
   error: {
      type: Array,
   },
   label: {
      type: String,
      required: true,
   },
   id: {
      type: String,
      required: true,
   },
   type: {
      type: String,
      default: 'text',
   },
   select: {
      type: Boolean,
      default: false,
   },
   option: {
      type: Array,
   },
})

const emit = defineEmits(['input'])

const inputHandler = (event) => {
   emit('input', event.target.value)
}

const inputPhoneHandler = (event) => {
   // Устанавливаем отформатированное значение в переменную phoneNumber
   emit('input','7' + event.target.value)
}

const getErrorMessage = (errorCode) => {
   switch (errorCode) {
      case 'Value is required':
         return 'Обязательное поле'
      case 'This field should be at least 11 characters long':
         return 'Минимум 11 цифр'
      case 'Value must be numeric':
         return 'Введите номер'
      default:
         return ''
   }
}
</script>

<style lang="scss" scoped>
label {
   display: block;
   font-size: 14px;
   margin-bottom: 4px;
}

span {
   color: red;
   font-size: 12px;
}

input[type='text'],
input[type='tel'],
input[type='date'],
select {
   width: 100%;
   padding: 0.5rem;
   font-size: 1rem;
   border: 2px solid #cccccc94;
   border-radius: 6px;
   cursor: pointer;
}

input:focus,
select:focus {
   outline: none;
}

.phone-field {
   display: flex;
   gap: 5px;
}

.phone-field__prefix {
   background: white;
   color: #cdcdcd;
   pointer-events: none;
   width: 40px !important;
}
</style>
