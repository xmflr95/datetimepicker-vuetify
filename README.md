# DatetimePicker-vuetify
DatetimePicker with Vuetify  
Vuetify 순정 기능을 이용하여 제작한 DatetimePicker입니다.  
기본 DatePicker에 TimePicker를 버튼으로 전환하여 선택할 수 있도록 제작하였습니다.

#### Update  
2022-05-03) v1.1.0 업데이트, 추가 옵션 및 날짜 입력 개선

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

  <!-- options setting 2 (month-picker) -->
  <datetime-picker
    @dateTime="getDatetime"
    label="Start Datetime"
    no-title
    month-only
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
  - **pre-icon**: "Use default prepend-icon in vuetify's v-text-fields"  
  - **no-title**: "set no title picker"  
  - **date-only**: "return date format `YYYY-mm-dd` and disabled time-picker"  
  - **month-only**: "Use month-picker"  
  - **default-time**: "Select date then set time auto `00:00:00`"  

> **Note**  
> You can add more basic options for `Vuetify`. From the child component, not the parent component.  
> `Vuetify`의 더 많은 옵션을 자식 컴포넌트에서 추가해서 사용 가능합니다(커스텀도 가능)  

