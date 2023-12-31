<!-- Ticking年度报告H5页面 -->

<template>
  <div v-if="data" class="carousel carousel-vertical h-screen w-full">
    <div class="relative">
      <!-- 背景图片，位于/1.png -->
      <div class="absolute top-0 left-0 right-0 bottom-0">
        <img src="/1.gif" alt="背景图片" class="w-full h-full object-contain" />
      </div>

      <!-- 在中央显示用户名，用户名是{{ data.name }}，白色字，要有阴影 -->
      <div
        class="absolute flex flex-col justify-center items-center gap-2 w-full h-full text-2xl font-bold text-white"
      >
        <NuxtImg
          :src="data?.avatarUrl"
          alt="头像"
          class="w-16 h-16 rounded-full"
        />
        <span class="drop-shadow">@{{ data?.name }}</span>
      </div>
    </div>
    <div class="relative">
      <div class="absolute top-0 left-0 right-0 bottom-0">
        <img src="/2.gif" alt="背景图片" class="w-full h-full object-contain" />
      </div>

      <!-- 注册日期 -->
      <div
        class="absolute flex justify-center items-center w-full h-full text-[2rem] font-bold text-black"
      >
        <span class="drop-shadow">{{ registerTimeText }}</span>
      </div>
    </div>
    <div class="relative" v-if="data.latestFocusTime">
      <div class="absolute top-0 left-0 right-0 bottom-0">
        <img src="/3.gif" alt="背景图片" class="w-full h-full object-contain" />
      </div>

      <!-- 睡得很晚日期 -->
      <div
        class="absolute flex flex-col justify-center items-center w-full h-full text-[2rem] font-bold text-white drop-shadow"
      >
        <p>{{ latestFocusTimeDayNumText }}</p>
        <p class="py-2 px-4 bg-red-500 rounded-full">
          {{ latestFocusTimeStartTimeText }}
        </p>
        <p>还在进行 {{ data.latestFocusTime?.name }}</p>
      </div>
    </div>
    <div class="relative">
      <!-- 年度计时 -->
      <div class="absolute top-0 left-0 right-0 bottom-0">
        <img src="/4.gif" alt="背景图片" class="w-full h-full object-contain" />
      </div>
      <div
        class="absolute flex flex-col justify-center items-center w-full h-full text-[2rem] text-black drop-shadow"
      >
        <p>今年总共计时</p>
        <p class="py-2 px-4 bg-red-500 text-white rounded-full">
          {{ (data.focusTimeTotalTime / 60 / 60 / 1000).toFixed(0) }}小时
        </p>

        <!-- 最多的计时名称 -->
        <div v-if="data.mostFocusTimeCount" class="pt-8 flex items-center flex-col">
          <p>最多计时的是</p>
          <p>{{ data.mostFocusTimeCount }}次的</p>
          <p class="py-2 px-4 bg-blue-500 text-center text-white rounded-full">
            {{ data.mostFocusTimeName }}
          </p>
        </div>
      </div>
    </div>
    <div class="relative" v-if="data.todoItemTotalCount != 0">
      <div class="absolute top-0 left-0 right-0 bottom-0">
        <img src="/5.gif" alt="背景图片" class="w-full h-full object-contain" />
      </div>

      <!-- 创建待办的数量 -->
      <div
        class="absolute flex flex-col justify-center items-center w-full h-full text-[2rem] font-bold text-black drop-shadow"
      >
        <p class="py-2 px-4 bg-red-500 text-white rounded-full">
          {{ data.todoItemTotalCount }}个待办
        </p>
      </div>
    </div>
    <div class="relative">
      <div class="absolute top-0 left-0 right-0 bottom-0">
        <img src="/6.gif" alt="背景图片" class="w-full h-full object-contain" />
      </div>

      <!-- 二维码 -->
      <div
        class="absolute flex flex-col pt-10 justify-center items-center w-full h-full text-[2rem] font-bold text-black drop-shadow"
      >
        <p>@{{ data.name }} 的年度报告</p>
        <div id="qrcode" ref="qrcode">
          <canvas id="canvas" ref="qrCanvas"></canvas>
        </div>
      </div>
    </div>
  </div>
  <div v-else>loading...</div>
</template>

<style>
.carousel > * {
  @apply carousel-item h-screen w-full;
}
</style>

<script lang="ts" setup>
import QRCode from "qrcode";
// 先获取query参数
const query = useRoute().query;

const res: any = await useFetch(
  "http://todo.i99yun.com/v2/report/annual?userId=" + query.userId
);

const data = res.data.value.data as UserData;
// yyyy年mm月d日
const registerTimeText = new Date(data.registerTime).toLocaleDateString(
  "zh-CN",
  {
    year: "numeric",
    month: "long",
    day: "numeric",
  }
);

// 转换为年月日日期
const latestFocusTimeDayNumText = new Date(
  data.latestFocusTime?.startTime
).toLocaleDateString("zh-CN", {
  year: "numeric",
  month: "long",
  day: "numeric",
});

// 时间
const latestFocusTimeStartTimeText = new Date(
  data.latestFocusTime?.startTime
).toLocaleTimeString("zh-CN", {
  hour12: false,
  // hour: "2-digit",
  minute: "2-digit",
  second: "2-digit",
});

onMounted(async () => {
  let canvas = document.getElementById("canvas");
  QRCode.toCanvas(
    canvas,
    "https://2023.oboard.eu.org/?userId=" + query.userId,
    { width: 200 },
    function (error: any) {
      if (error) console.error(error);
      // console.log("success!");
    }
  );
});

type SimpleUserInfo = {
  userId: number;
  name: string;
  avatarUrl: string;
};

type FocusTimeInfo = {
  simpleUserInfo: SimpleUserInfo | null;
  uuid: string;
  name: string;
  userId: number;
  startTime: number;
  endTime: number;
  pauseStartTime: number;
  pauseEndTime: number;
  scheduledTime: number;
  type: number;
  pauseTotalTime: number;
  dayNum: number;
  state: number;
  createTime: number;
  updateTime: number;
  isDeleted: number;
  timeTotal: number;
};

type UserDataResponse = {
  status: number;
  data: UserData;
  message: string;
};

type UserData = {
  userId: number;
  name: string;
  avatarUrl: string;
  registerTime: number;
  focusTimeTotalTime: number;
  focusTimeTotalCount: number;
  todoItemTotalCount: number;
  todoListTotalCount: number;
  latestFocusTime: FocusTimeInfo;
  mostFocusDayNum: number;
  mostFocusDayCount: number;
  mostFocusTimeName: string;
  mostFocusTimeCount: number;
};
// 从http://localhost/v2/report/annual?userId=3获取数据
</script>
