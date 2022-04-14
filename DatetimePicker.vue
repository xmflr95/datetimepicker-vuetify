<template>
    <div>
      <v-menu
        ref="menu"
        v-model="menu"
        :close-on-content-click="false"
        transition="scale-transition"
        offset-y        
        min-width="auto"
        max-width="290"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-text-field
            v-model="datetimeFormatted"
            :label="label"
            persistent-hint
            color="blue darken-3"
            v-bind="attrs"
            @blur="getDateTime"
            @keypress.enter="evtTextEnter($event)"
            v-on="on"
            :prepend-icon="computedPreIcon"
            :disabled="disabled"
            :readonly="readonly"
          ></v-text-field>
        </template>
        <template v-if="select.dateMenu">
          <v-date-picker
            v-model="computedDateTime.date"
            color="blue darken-1"
            header-color="blue darken-1"
            :no-title="noTitle"
            @input="inputDate"
            :weekday-format="getDay"
            :month-format="getMonth"
            :title-date-format="getTitleDate"
            :header-date-format="getHeaderTitleMonth"
          ></v-date-picker>
        </template>
        <template v-if="select.timeMenu">
          <v-time-picker
            v-model="computedDateTime.time"
            scrollable
            use-seconds
            color="blue darken-1"
            header-color="blue darken-1"
            @change="inputTime"
          ></v-time-picker>
        </template>
        <template>
          <v-btn
            block
            elevation="2"
            @click="clickChangeBtn"
          >{{ btnContent }}</v-btn>
        </template>
      </v-menu>
    </div>
</template>

<script>

export default {
  name: 'DatetimePicker',
  props: {
    label: String,
    preIcon: Boolean,
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  components: {
  },
  data() {
    return {
      noTitle: false,
      // locale: 'ko-KR', // 날짜 뒤에 '일'이 붙어서 커스텀 필요
      btnContentData: ['날짜 선택', '시간 선택'],
      daysOfWeek: ['일', '월', '화', '수', '목', '금', '토'],
      monthName: ['1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월', '11월', '12월'],
      btnContent: '시간 선택',

      menu: false,  // picker 활성화 여부    
      
      // datetime format 확인용
      regDate: RegExp(/^\d{4}-(0[1-9]|1[012])-(0[1-9]|[12][0-9]|3[01]) (0[1-9]|1[0-9]|2[0-4]):(0[1-9]|[1-5][0-9]):(0[1-9]|[1-5][0-9])$/),
      // 날짜/시간 피커 열림 확인
      select: {
        dateMenu: true,
        timeMenu: false,
      },
      nowDateISOString: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString(),
      nowDateObj: (new Date(Date.now())),

      // 날짜/시간 선택 메뉴 여부 확인용
      datetime: {
        date: '',
        time: '',
      },
      datetimeFormatted: '', // v-text-field 표시 데이터
    };
  },
  computed: {
    computedDateTime: {
      get() {
        if (!this.datetime.date) { // 날짜 설정
          const formatDate = this.nowDateISOString.substr(0, 10);
          this.datetime = {...this.datetime, date: this.parseDate(formatDate) };
        }
        if (!this.datetime.time) { // 시간 설정
          this.datetime = { ...this.datetime, time: this.parseTime(this.nowDateObj) };
        }

        return this.datetime;
      },
      set(_datetime) {
        // _datetime = { date: String, time: String } : Object
        this.datetime = { ...this.datetime, ..._datetime };
      },
    },
    computedPreIcon() {
      return this.preIcon ? "mdi-calendar": null;
    }
  },
  watch: {
    // 텍스트 필드값을 감시하여 실제 시간 Object값을 변경
    datetimeFormatted(_date) {
      if (this.regDate.test(_date)) {
        // console.log('datetimeFormatted : ', _date);
        const [date, time] = _date.split(" ");
        this.datetime = { ...this.datetime, date: date, time: time };
      } else {
        this.datetime = { ...this.datetime };
      }
    }
  },
  methods: {
    parseDate(date) {
      // console.log("parse", date);
      if (!date) return null;

      const [year, month, day] = date.split('-');
      return `${year.padStart(4, '0')}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`;
    },
    parseTime(date) {
      // console.log("parse Time", date);
      const hour = date.getHours();
      const min = date.getMinutes();
      const sec = date.getSeconds();
      // console.log(`${hour}:${min}:${sec}`);

      return `${hour.toString().padStart(2, '0')}:${min.toString().padStart(2, '0')}:${sec.toString().padStart(2, '0')}`;
    },
    formatDatetime(strDatetime) {
      if (!strDatetime) return null;
      const [date, time] = strDatetime.split(' ');
      const [year, month, day] = date.split('-');
      const [hour, min, sec] = time.split(':');

      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')} ${hour.padStart(2, '0')}:${min.padStart(2, '0')}:${sec.padStart(2, '0')}`;
    },

    /**
     * 캘린더 view 커스텀 부분
     */
    getDay(date) {
      const i = new Date(date).getDay(date);
      return this.daysOfWeek[i];
    },
    getMonth(date) {
      const i = new Date(date).getMonth(date);
      return this.monthName[i];
    },
    getHeaderTitleMonth(date) {
      const year = new Date(date).getFullYear(date);
      const month = new Date(date).getMonth(date);
      return `${year}년 ${this.monthName[month]}`;
    },
    getTitleDate(date) {      
      const month = new Date(date).getMonth(date);
      const day = new Date(date).getDate(date);
      const week = new Date(date).getDay(date);
      return `${this.monthName[month]} ${day}일 ${this.daysOfWeek[week]}요일`;
    },

    // enter 이벤트 blur로 넘김
    evtTextEnter(e) {
      if (this.menu) {
        this.menu = false; // 피커 닫기
      }
      e.target.blur();
    },

    /**
     * 버튼 이벤트(시간/날짜 선택)
     */
    clickChangeBtn() {      
      if (this.select.dateMenu) {
        this.select = { ...this.select, dateMenu: false, timeMenu: true };
        this.btnContent = this.btnContentData[0];
      } else {
        this.select = { ...this.select, dateMenu: true, timeMenu: false };
        this.btnContent = this.btnContentData[1];
      }
    },
    inputDate(date) {
      // console.log("inputDate : ", date);
      this.datetimeFormatted = this.setDatetimeText(this.datetime);
      this.menu = false; // 피커 닫기
      // this.$emit('dateTime', dateTime); // 상위 컴포넌트로 전달
      this.emitDateTime(this.datetimeFormatted); // 상위 컴포넌트로 전달
    },

    getDateTime() {
      this.datetimeFormatted = this.setDatetimeText(this.datetime);
      this.emitDateTime(this.datetimeFormatted); // 상위 컴포넌트로 전달
    },
    inputTime(tdata) {
      this.computedDateTime = { time: tdata };
      this.menu = false;
      this.datetimeFormatted = this.setDatetimeText(this.datetime);
      this.emitDateTime(this.datetimeFormatted); // 상위 컴포넌트로 전달
    },

    /**
     * 상위 컴포넌트로 datetime 값 전달
     */
    emitDateTime(datetime) {
      this.$emit('dateTime', datetime);
    },

    /**
     * 데이터 객체를 문자열 규칙으로 변경
     * @param datetime: Object
     * @return datetime: string
     */
    setDatetimeText(datetime) {
      const { date, time } = datetime;
      const [year, month, day] = date.split("-");
      const [hour, min, sec] = time.split(":");
      // console.log(`spread date : ${year}-${month}-${day} ${hour}:${min}:${sec}`);
      return `${year.padStart(4, '0')}-${month.padStart(2, '0')}-${day.padStart(2, '0')} ${hour.padStart(2, '0')}:${min.padStart(2, '0')}:${sec.padStart(2, '0')}`;
    },
  },
};
</script>

<style lang="scss">
  .v-date-picker-title__date {
    overflow: visible;
  }

  .v-time-picker-title__time  {
    .v-picker__title__btn {
      font-size: 56px
    }
    span {
      font-size: 56px
    }
  }
</style>
