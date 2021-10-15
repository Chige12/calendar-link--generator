<!-- Please remove this file from your project -->
<template>
  <div class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      <div class="mt-8 bg-white overflow-hidden shadow sm:rounded-lg p-6">
        <h2 class="text-xl leading-7 font-semibold">
          Google Calendar - Link Generator
        </h2>
        <a-divider />
        <div class="mb-6">
          <p class="mb-2">予定のタイトル</p>
          <a-input v-model="title" placeholder="予定のタイトル" size="large" />
        </div>
        <div class="mb-6">
          <p class="mb-2">予定の説明</p>
          <a-textarea v-model="details" placeholder="予定の説明" class="mb-1" :rows="2"/>
          <a-input v-model="link" placeholder="会議のリンク">
            <a-icon slot="prefix" type="link" />
          </a-input>
        </div>
        <div class="mb-6">
          <p class="mb-2">日付</p>
          <a-date-picker @change="dateChange"/>
        </div>
        <div class="flex items-end mb-12">
          <div>
            <p class="mb-2">開始時間</p>
            <a-time-picker :default-value="moment('12:00', 'HH:mm')" format="HH:mm" @change="startTimeChange" />
          </div>
          <span class="mx-2 py-1.5">〜</span>
          <div>
            <p class="mb-2">終了時間</p>
            <a-time-picker :default-value="moment('13:00', 'HH:mm')" format="HH:mm" @change="endTimeChange" />
          </div>
        </div>
        <div>
          <p class="mb-2">生成URL</p>
          <a-textarea id="genUrl" type="text" :value="generateCalendarUrl" class="mb-2" :rows="3"/>
        </div>
      </div>
      <div class="flex justify-center pt-4 space-x-1">
        <span>Created by</span><a href="https://twitter.com/Chige12_">Chige12</a>
      </div>
    </div>
  </div>
</template>
<script>
import moment from 'moment';
export default {
  data() {
    return {
      title: 'インターン生のみランチ会',
      details: '',
      link: '',
      date: '',
      startTime: '12:00',
      endTime: '13:00',
      genUrl: ''
    }
  },
  computed: {
    generateCalendarUrl() {
      return this.generateUrl()
    }
  },
  methods: {
    moment,
    dateChange(_, date) {
      this.date = date
    },
    startTimeChange(_, startTime) {
      this.startTime = startTime
    },
    endTimeChange(_, endTime) {
      this.endTime = endTime
    },
    isAbleGenUrl() {
      return this.date && this.title && this.startTime && this.endTime
    },
    generateUrl() {
      if (!this.isAbleGenUrl()) return '';

      const startDateStr = `${this.date}T${this.startTime}:00+09:00`
      const endTimeStr = `${this.date}T${this.endTime}:00+09:00`
      const link = this.link ? `Meeting link: ${this.link}` : ''
      const details = `${this.details}\n${link}`
      const location = ''

      const genUrl = 'http://www.google.com/calendar/event?' +
        'action='   + 'TEMPLATE' +
        '&text='    + encodeURIComponent(this.title) +
        '&details=' + encodeURIComponent(details) +
        '&location='+ encodeURIComponent(location) +
        '&dates='   + this.getUTC(startDateStr) + '/' + this.getUTC(endTimeStr) +
        '&trp='     + 'false'

      this.genUrl = genUrl
      return genUrl
    },
    getUTC(dateStr) {
    	const date = new Date(dateStr);
    	return date.getUTCFullYear() +
    		this.zerofill(date.getUTCMonth()+1) +
    		this.zerofill(date.getUTCDate()) +
    		'T' +
    		this.zerofill(date.getUTCHours()) +
    		this.zerofill(date.getUTCMinutes()) +
    		this.zerofill(date.getUTCSeconds()) +
    		'Z';
    },
    zerofill(num) {
    	return ('0'+num).slice(-2);
    },
  }
};
</script>