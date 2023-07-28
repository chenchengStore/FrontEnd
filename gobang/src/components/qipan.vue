<template>
  <div class="main" @click="chess" @mousemove="move">
    <qizi name="qizi"></qizi>
    <div class="line1" v-for="e in a" :key="e">
      <div class="line2" v-for="x in a" :key="x"></div>
    </div>
  </div>
</template>

<script setup>
import qizi from "./qizi";
let index = 0;
const a = Array.apply(null, { length: 21 }).map(function (item) {
  item = index;
  index += 1;
  return item;
});
// 设置棋子的颜色 0为黑色，1为白色
let flag = 0;
// 以下将棋盘及棋子抽象为两个20*20的数组，分别为黑棋落子和白棋落子
const offset = 155
const qiziArr = [[], []];
console.log(qiziArr);
for (let i = 0; i < 2; i++) {
  for (let j = 0; j < 20; j++) {
    let Temp = Array.apply(null, { length: 20 }).map(function (item) {
      item = 0;
      return item;
    });
    qiziArr[i].push(Temp);
  }
}

// 点击落子的各种判断
function chess(event) {
  const qizi = document.getElementsByClassName("qizi");
  const newqizi = document.createElement("div");
  const main = document.getElementsByClassName("main")[0];
  // 获得当前点击落子的位置
  const i = parseInt(qizi[qizi.length - 1].offsetTop / 40 - 1);
  const j = parseInt((qizi[qizi.length - 1].offsetLeft - offset) / 40 - 1);
  // 设置落子的样式和位置
  newqizi.className = "qizi";
  newqizi.style.left = qizi[qizi.length - 1].style.left;
  newqizi.style.top = qizi[qizi.length - 1].style.top;
  // 判断是否重复落子
  if (qiziArr[0][i][j] === 1 || qiziArr[1][i][j] === 1) {
    alert("不能重复落子");
    return;
  }
  qiziArr[flag][i][j] = 1;
  newqizi.style.backgroundColor = qizi[qizi.length - 1].style.backgroundColor;
  main.insertBefore(newqizi, qizi[qizi.length - 1]);
  // console.log(qiziArr[flag]);

  // 判断胜利条件
  let win = [0, 0, 0, 0];
  // 竖
  for (let k = 1; k < 5; k++) {
    if (i - k >= 0) {
      if (qiziArr[flag][i - k][j] === 1) {
        win[0] += 1;
      } else {
        break;
      }
    }
  }
  for (let k = 1; k < 5; k++) {
    if (i + k < 20) {
      if (qiziArr[flag][i + k][j] === 1) {
        win[0] += 1;
      } else {
        break;
      }
    }
  }
  // 竖
  for (let k = 1; k < 5; k++) {
    if (j - k >= 0) {
      if (qiziArr[flag][i][j - k] === 1) {
        win[1] += 1;
      } else {
        break;
      }
    }
  }
  for (let k = 1; k < 5; k++) {
    if (j + k < 20) {
      if (qiziArr[flag][i][j + k] === 1) {
        win[1] += 1;
      } else {
        break;
      }
    }
  }
  // 二四象限
  for (let k = 1; k < 5; k++) {
    if (j - k >= 0 && i - k >= 0) {
      if (qiziArr[flag][i - k][j - k] === 1) {
        win[2] += 1;
      } else {
        break;
      }
    }
  }
  for (let k = 1; k < 5; k++) {
    if (j + k < 20 && i + k < 20) {
      if (qiziArr[flag][i + k][j + k] === 1) {
        win[2] += 1;
      } else {
        break;
      }
    }
  }
  // 一三象限
  for (let k = 1; k < 5; k++) {
    if (j - k >= 0 && i + k < 20) {
      if (qiziArr[flag][i + k][j - k] === 1) {
        win[3] += 1;
      } else {
        break;
      }
    }
  }
  for (let k = 1; k < 5; k++) {
    if (j + k < 20 && i - k >= 0) {
      if (qiziArr[flag][i - k][j + k] === 1) {
        win[3] += 1;
      } else {
        break;
      }
    }
  }

  if (win[0] >= 4 || win[1] >= 4 || win[2] >= 4 || win[3] >= 4) {
    if (flag === 0) {
      alert("黑方胜利");
    }
    if (flag === 1) {
      alert("白方胜利");
    }
    const len = qizi.length;
    for (let index = 1; index < len; index++) {
      main.removeChild(qizi[0]);
    }
    for (let m = 0; m < qiziArr.length; m++) {
      for (let n = 0; n < qiziArr[0].length; n++) {
        for (let l = 0; l < qiziArr[0][0].length; l++) {
          qiziArr[m][n][l] = 0;
        }
      }
    }
    return;
  }

  if (qizi[qizi.length - 1].style.backgroundColor === "black") {
    qizi[qizi.length - 1].style.backgroundColor = "white";
    flag = 1;
  } else if (qizi[qizi.length - 1].style.backgroundColor === "white") {
    flag = 0;
    qizi[qizi.length - 1].style.backgroundColor = "black";
  }
  console.log(qiziArr);
}
function move(event) {
  const qizi = document.getElementsByName("qizi");
  
  if (event.clientX >= offset && event.clientX <= 1990)
    qizi[qizi.length - 1].style.left =
      event.clientX - qizi[qizi.length - 1].offsetWidth / 2 + "px";
  if (event.clientY >= 0 && event.clientY <= 840)
    qizi[qizi.length - 1].style.top =
      event.clientY - qizi[qizi.length - 1].offsetHeight / 2 + "px";
  const left = qizi[qizi.length - 1].offsetLeft;
  const top = qizi[qizi.length - 1].offsetTop;
  let xLeft = -((left - offset) % 40);
  let xTop = -(top % 40);

  if ((left - offset) % 40 > 20) {
    xLeft = 40 - ((left - offset) % 40);
  }

  if (top % 40 > 20) {
    xTop = 40 - (top % 40);
  }

  qizi[qizi.length - 1].style.left = left + xLeft + "px";
  qizi[qizi.length - 1].style.top = top + xTop + "px";
}
</script>

<style>
body {
  display: flex;
  align-items: center;
  justify-content: center;
}

.main {
  border-left: solid 1px black;
  border-top: solid 1px black;
  width: 840px;
  height: 840px;
  background-color: rgb(234, 188, 121);
}
.line1 {
  float: top;
  height: 39px;
  border-bottom: solid 1px black;
}
.line2 {
  float: left;
  width: 39px;
  height: 40px;
  border-right: solid 1px black;
}
.qizi {
  position: absolute;
  height: 20px;
  width: 20px;
  border-radius: 100%;
  background-color: black;
}
</style>
