<template>
    <div>
      <!-- 
        option
        @dateTime: "datetime data from children emit"
        label: "v-text label option"
        disabled: "disable option"
        preIcon: "default set vuetify datepicker icon"
       -->
      <datetime-picker
        @dateTime="getDateEndTime"
        label="종료일자"        
      />
    </div>
</template>


<script>
// Link Components dir
import DatetimePicker from '@/DatetimePicker';

export default {
  name: 'Sample',
  components: {
    DatetimePicker,
  },
  computed: {
  },    
  methods: {
    // get Datetime data from children
    getDateStartTime(dateStartTime) {
      this.dateStartTime = dateStartTime;
    },
    getDateEndTime(dateEndTime) {
      this.dateEndTime = dateEndTime;
    }
  },
  data() {
    return {
      // set Data
      dateStartTime: '',
      dateEndTime: '',
    };
  },
};
</script>
