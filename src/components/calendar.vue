<style lang="less">
.block-calendar {
  padding: 15px;
  .calendar-header {
    display: flex;
    align-items: center;
    justify-content: space-between;

    .item-date-calendar {
      color: #333;
      text-align: left;
      letter-spacing: -0.1em;
      font-weight: bold;
      font-size: 18px;
      font-family: Michroma, "Segoe UI Light", "Segoe UI", "Segoe UI WP",
        "Microsoft Jhenghei", "微软雅黑", sans-serif, Times;
      line-height: 1.5;
    }
    .item-subdate-calendar {
      margin: 0 0 8px 0;
      color: #666;
      text-align: left;
      letter-spacing: -0.05em;
      font-style: italic;
      font-family: Michroma, "Segoe UI Light", "Segoe UI", "Segoe UI WP",
        "Microsoft Jhenghei", "微软雅黑", sans-serif, Times;
      line-height: 1.2;
    }
    .item-sign-calendar {
      text-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
      font-weight: bold;
      font-size: 28px;
      font-family: "Consolas", "Microsoft Yahei", Arial, sans-serif;
      line-height: normal;
      cursor: pointer;

      user-select: none;
    }
  }
  .table-box {
    .good,
    .bad {
      display: flex;
      align-items: stretch;
      justify-content: space-between;
      .l {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 30%;
        height: auto;
        color: #222;
        text-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
        font-weight: bold;
        font-size: 32px;
        line-height: normal;
      }
      .r {
        width: 70%;
        li {
          display: flex;
          align-items: center;
          padding: 7px 10px;
          height: 50px;
          transition: all 0.5s;
          &:hover {
            padding-left: 20px;
            background-color: rgba(0, 0, 0, 0.07);
          }
          .a {
            margin-bottom: 5px;
            color: #444;
            font-weight: 400;
            font-size: 18px;
          }
          .b {
            color: #777;
            font-style: italic;
            font-size: 12px;
          }
          .c {
            display: block;
            margin-right: 10px;
            height: 100%;
          }
        }
      }
    }
    .good {
      background-color: #ffa;
      .l {
        background-color: #fe4;
      }
    }
    .bad {
      background-color: #ffddd3;
      .l {
        background-color: #f44;
        color: #fff;
      }
    }
  }
}
</style>

<template>
  <div class="block-calendar">
    <!--  -->
    <div class="calendar-header">
      <div>
        <p class="item-date-calendar">
          <span>{{date.yy}}年{{date.mm}}月{{date.dd}}日</span>
          <span>{{date.sweek}}</span>
        </p>
        <p class="item-subdate-calendar">
          <span>{{date.lunar}}</span>
        </p>
      </div>
      <div>
        <template v-if="vIf">
          <el-date-picker size="mini" v-model="birthday" format="yyyy 年 MM 月 dd 日" value-format="yyyyMMdd" type="date" placeholder="选择你的生日"></el-date-picker>
          <el-button size="mini" @click="onClick">确认</el-button>
        </template>
        <p v-else @dblclick="dblClick" class="item-sign-calendar" :style="`color:${luck.color}`" :data-balloon="`今日运势指数：${luck.attr}%`" data-balloon-pos="left">{{luck.label}}</p>
      </div>
    </div>
    <!--  -->
    <div class="table-box">
      <div class="good">
        <div class="l">
          <span>宜</span>
        </div>
        <ul class="r">
          <li v-for="item in fortune.good">
            <img class="c" :src="`./static/img/${item.img}.gif`" alt="">
            <div>
              <p class="a">{{item.label}}</p>
              <p class="b">{{item.good}}</p>
            </div>
          </li>
        </ul>
      </div>
      <div class="bad">
        <div class="l">
          <span>忌</span>
        </div>
        <ul class="r">
          <li v-for="item in fortune.bad">
            <img class="c" :src="`./static/img/${item.img}.gif`" alt="">
            <div>
              <p class="a">{{item.label}}</p>
              <p class="b">{{item.bad}}</p>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import getDay from "../util/LunarDay.js";
import Luck from "../util/Luck.js";
import Fortune from "../util/Fortune.js";

export default {
  data() {
    return {
      date: {},
      luck: "",
      fortune: {},
      birthday: localStorage.getItem("ACER_BIR") || "",
      vIf: true,
      setDate: {
        yy: new Date().getFullYear(),
        mm: new Date().getMonth() + 1,
        dd: new Date().getDate()
      }
    };
  },
  // computed: {
  //   Abirthday() {
  //     return localStorage.getItem("ACER_BIR");
  //   }
  // },
  methods: {
    onClick() {
      this.vIf = false;
      localStorage.setItem("ACER_BIR", this.birthday);
    },
    dblClick() {
      this.vIf = true;
    },
    beforeDay() {
      this.setDate.dd;
    },
    afterDay() {}
  },
  mounted() {
    if (this.birthday) {
      this.vIf = false;
    }
    console.log(this.birthday);
    this.date = getDay(this.setDate.yy, this.setDate.mm, this.setDate.dd);
    this.fortune = Fortune(this.setDate.yy, this.setDate.mm, this.setDate.dd);
    this.luck = Luck(this.birthday);
  },
  watch: {
    birthday() {
      this.luck = Luck(this.birthday);
    },
    setDate() {
      this.date = getDay(this.setDate.yy, this.setDate.mm, this.setDate.dd);
      this.fortune = Fortune(this.setDate.yy, this.setDate.mm, this.setDate.dd);
    }
  }
};
</script>


