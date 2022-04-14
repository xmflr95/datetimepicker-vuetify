# DatetimePicker-vuetify
DatetimePicker with Vuetify  
Vuetify 순정 기능을 이용하여 제작한 DatetimePicker입니다.  
기본 DatePicker에 TimePicker를 버튼으로 전환하여 선택할 수 있도록 제작하였습니다.

## Link  
  - [Vuetify (Official WebSite)](https://vuetifyjs.com/en/)
  - [Vuetify-Date Pickers](https://vuetifyjs.com/en/components/date-pickers/)
  - [Vuetify-Time Pickers](https://vuetifyjs.com/en/components/time-pickers/)
  - [Vuetify-Text Field](https://vuetifyjs.com/en/components/text-fields/)


## DatetimePicker
### Example  
```html
<template>
  <!-- default -->
  <datetime-picker />

  <!-- options setting -->
  <datetime-picker
    @dateTime="getDatetime"
    label="Start Datetime"
    preIcon
  >
  </datetime-picker>
</template>

<script>
  // vue settings...
  data() {
    return {
      datetime: '',
    }
  },
  method: {
    getDatetime(datetime) {
      this.datetime = datetime; // Binds data from emit  a child-component
    }
  }
  // ...vue settings
</script>
```
###### options:  
  - **@dateTime**: 자식컴포넌트로부터 `datetime(:string)`을 받아옴, "Options to import `datetime(:String)` from child components"
  - **label**: v-text의 label 옵션, "v-text label option"
  - **disabled**: "disable option"
  - **preIcon**: "Use default prepend-icon in vuetify's v-text-fields"  

> **Note**  
> You can add more basic options for `Vuetify`. From the child component, not the parent component.  
> `Vuetify`의 더 많은 옵션을 자식 컴포넌트에서 추가해서 사용 가능합니다(커스텀도 가능)  

