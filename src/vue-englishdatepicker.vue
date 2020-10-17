<template>
  <div class="datepicker" @click.stop>
    <input
      type="text"
      :class="this.classValue"
      v-model="formatedValue"
      @focus="show()"
      :placeholder="this.placeholder"
    />
    <div v-if="visible" :class="['calendar', { show: visible }]">
      <div class="calendar__header">
        <div class="calendar__year">{{ formatedYear }}</div>
        <div class="calendar__date">{{ formatedDate }}</div>
      </div>
      <div class="calendar__body">
        <!-- month -->
        <div class="calendar__month">
          <button class="calendar__month__prev" @click="prev()">
            <b>></b>
          </button>
          <span>{{ formatedYearOrMonth }}</span>
          <select
            @change="monthSelectChange"
            v-model="monthValue"
            v-if="this.monthSelect"
          >
            <option
              style="text-align-last: center"
              v-for="(month, index) in getMonthsList"
              :key="month"
              :label="month"
              :value="index"
            >
              <!-- {{ month }} -->
            </option>
          </select>
          <select
            @change="yearSelectChange"
            v-model="yearValue"
            v-if="this.yearSelect"
            style="margin-left: 5px"
          >
            <option
              style="text-align-last: center"
              v-for="i in numberofYears"
              :key="i"
              :value="startingYear + (i - 1)"
              :label="startingYear + (i - 1)"
            ></option>
          </select>
          <button @click="next()"><b>></b></button>
        </div>
        <!-- week days -->
        <div style="padding: 3px">
          <div class="calendar__weeks">
            <div
              class="calendar__weekday"
              v-for="(weekday, w) in weekdays"
              :key="w"
            >
              {{ weekday }}
            </div>
          </div>
          <!-- days of month -->
          <div class="calendar__days">
            <div
              class="calendar__day_spacer"
              :style="{ gridColumn: `span ${startweek}` }"
            ></div>
            <div
              :class="[
                'calendar__day',
                { selected: active(day) },
                { today: checkToday(day) },
              ]"
              v-for="(day, d) in days"
              :key="d"
              @click="select(day)"
            >
              {{ day.date() }}
            </div>
          </div>
        </div>
      </div>
      <div class="calendar__footer">
        <button @click="today()">Today</button>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "VueEnglishdatepicker", // vue component name
  props: {
    value: { type: String, default: "" },
    format: { type: String, default: "YYYY-MM-DD" },
    classValue: { type: String, default: "" },
    placeholder: { type: String, default: "" },
    yearSelect: { type: Boolean, default: true },
    monthSelect: { type: Boolean, default: true },
  },
  model: {
    event: "change",
  },
  data() {
    return {
      date: moment(this.value || moment(), this.format),
      formatedValue: this.value,
      visible: false,
      startingYear: 1940,
      numberofYears: 100,
      endDay: null,
      yearValue: this.value == "" ? moment().year() : moment(this.value).year(),
      monthValue:
        this.value == "" ? moment().month() : moment(this.value).month(),
      startMonthValue: null,
      currentDateValue: undefined,
    };
  },
  computed: {
    getMonthsList() {
      return moment.months();
    },
    year() {
      return this.date.year();
    },
    month() {
      return this.date.month();
    },
    weekdays() {
      return moment.weekdaysMin();
    },
    days() {
      const endDay = this.date
        .clone()
        .endOf("month")
        .date();
      return Array(endDay)
        .fill()
        .map((_, idx) => moment([this.year, this.date.month(), idx + 1]));
    },
    startweek() {
      return this.date
        .clone()
        .startOf("month")
        .day();
    },
    formatedYear() {
      return this.date.format("YYYY");
    },
    formatedYearOrMonth() {
      if (this.monthSelect == false && this.yearSelect == false) {
        return this.date.format("MMMM YYYY");
      }

      if (this.monthSelect == false) {
        return this.date.format("MMMM");
      }

      if (this.yearSelect == false) {
        return this.date.format("YYYY");
      }
      return "";
    },
    formatedDate() {
      return this.date.format("dddd, DD MMMM");
    },
  },
  methods: {
    active(date) {
      return this.date.unix() === date.unix();
    },
    monthSelectChange() {
      this.date = moment([this.year, this.monthValue]);
    },
    yearSelectChange() {
      this.date = moment([this.yearValue, this.month]);
    },
    next() {
      let _month = this.date.month() + 1;
      let _year = this.year;
      if (_month > 11) {
        _year++;
        _month = 0;
      }
      this.setMonthAndYear(_month, _year);
      this.date = moment([_year, _month]);
    },
    prev() {
      let _month = this.date.month() - 1;
      let _year = this.year;
      if (_month < 0) {
        _year--;
        _month = 11;
      }
      this.setMonthAndYear(_month, _year);
      this.date = moment([_year, _month]);
    },
    select(date) {
      this.date = date;
      this.formatedValue = this.date.format(this.format);
      this.$emit("change", this.formatedValue);
      this.hide();
    },
    show() {
      this.visible = true;
      setTimeout(() => document.addEventListener("click", this.hide), 200);
    },
    hide() {
      this.visible = false;
      document.removeEventListener("click", this.hide);
    },
    today() {
      this.select(moment());
      this.setMonthAndYear(this.date.month(), this.date.year());
    },
    checkToday(date) {
      let today = moment();
      return (
        date.date() == today.date() &&
        date.year() == today.year() &&
        date.month() == today.month()
      );
    },
    setMonthAndYear(month, year) {
      this.monthValue = month;
      this.yearValue = year;
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  box-sizing: border-box;
  font-family: "Open Sans", sans-serif;
}
.datepicker {
  position: relative;
}
.datepicker button {
  outline: none;
  border: 0;
  background: transparent;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}
.calendar {
  z-index: 9;
  position: absolute;
  width: 260px;
  top: 100%;
  box-shadow: 0px 14px 45px rgba(0, 0, 0, 0.25),
    0px 10px 18px rgba(0, 0, 0, 0.22);
  background: #fff;
  visibility: hidden;
  opacity: 0;
  /* transform: translateY(-50%) translateX(50%); */
  /* transition: all 0.3s linear; */
}
.calendar.show {
  visibility: visible;
  opacity: 1;
  /* transform: translateY(0px); */
}
.calendar__header {
  padding: 15px 10px;
  /* border-radius: 10px; */
  background: #5495c5;
  color: #fff;
}
.calendar__year {
  opacity: 0.6;
  font-size: 1rem;
  line-height: 1.2rem;
}
.calendar__date {
  font-size: 1.2rem;
  line-height: 1.5rem;
}
.calendar__month {
  padding: 5px 3px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.calendar__month select {
  height: 28px;
  width: 100px;
  border-radius: 5px;
  border-color: #7ca3f1;
  text-align-last: center;
}
.calendar__month button {
  width: 25px;
  margin-right: 4px;
  height: 28px;
  margin-left: 4px;
  border-radius: 5px;
  color: white;
  text-align: center;
  background: #247ac4;
}
.calendar__month button:hover {
  background: rebeccapurple;
}

.calendar__month__prev {
  transform: rotate(180deg);
}
.calendar__weeks,
.calendar__days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}
.calendar__days {
  gap: 4px;
}
.calendar__day,
.calendar__weekday {
  text-align: center;
  font-size: 12px;
}
.calendar__weekday {
  opacity: 0.8;
  font-weight: 300;
}
.calendar__day {
  width: 32px;
  height: 32px;
  line-height: 32px;
  font-weight: 300;
  color: #1c94b8;
  font-weight: bold;
  cursor: pointer;
  border-radius: 5px;
  background: #dfeffc;
  /* transition: all 0.3s ease-in-out; */
}
.calendar__day.selected {
  background: rebeccapurple;
  color: #fff;
}
.calendar__day.today {
  background: #f77777;
  color: #fff;
}
.calendar__day:hover {
  background: rebeccapurple;
  color: #fff;
  opacity: 0.8;
}
.calendar__footer {
  text-align: right;
}
.calendar__footer button {
  padding: 8px 10px;
  text-transform: uppercase;
  font-weight: bold;
  color: rebeccapurple;
  opacity: 0.9;
}
.calendar__footer button:hover {
  opacity: 1;
}
</style>
