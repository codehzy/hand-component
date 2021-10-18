<template>
  {{ cells }}
  <div>
    <table>
      <tr v-for="(headers, index) in cells" :key="index">
        <th class="title-divide">
          {{ headers.title }}
        </th>
      </tr>
      <tr>
        <td>123</td>
        <td>123</td>
      </tr>
    </table>
  </div>
</template>

<script lang="tsx">
import { defineComponent } from "vue";

/**
 * 表具体内容
 */
interface Row {
  IntroName: string;
  index?: string;
  formate: (value: string) => string;
  values: number[];
}

/**
 * 表内容
 */
interface Cell {
  title: string;
  rows: Row[];
}

/**
 * 表头
 */
interface TableScoll {
  headers: string[];
  cells: Cell[];
}
type FormateStep = "cn_yi" | "cn_yuan";

/**
 * 格式化亿元
 */
const forMateValue = (value: string | number, step: FormateStep): string => {
  // 十亿
  let res = "";

  const cnYiFunc = () => {
    res = (Number(value) / 100000000).toFixed(2) + `亿元`;
    return res;
  };
  const cnYuanFunc = () => {
    res = value + `元`;
    return res;
  };

  switch (step) {
    case "cn_yi":
      cnYiFunc();
      break;
    case "cn_yuan":
      cnYuanFunc();
      break;
    default:
      break;
  }
  return res;
};

// 构建自己要渲染的数据
export const cells: TableScoll["cells"] = [
  {
    title: "关键指标",
    rows: [
      {
        IntroName: "营业总收入",
        index: "income",
        values: [],
        formate: (value: string): string => {
          // 亿
          const showValue = forMateValue(value, "cn_yi");
          return showValue;
        },
      },
    ],
  },
  {
    title: "指标提醒",
    rows: [
      {
        IntroName: "营业利润",
        index: "income",
        values: [],
        formate: (value: string): string => {
          // 亿
          const showValue = forMateValue(value, "cn_yi");
          return showValue;
        },
      },
    ],
  },
];

const tableData = [
  {
    year: 2020,
    income: 1200000000,
    lirun: 0.5,
  },
  {
    year: 2022,
    income: 188800000000,
    lirun: 0.6,
  },
].sort((item) => item.year);

for (const yearData of tableData) {
  for (const [key, value] of Object.entries(yearData)) {
    for (let i = 0; i < cells.length; i++) {
      const cur = cells[i];
      const targetValueIndex = cur.rows.findIndex((item) => item.index === key);
      // 找到了
      if (targetValueIndex != -1) {
        cur.rows[targetValueIndex].values.push(value);
        continue;
      }
    }
  }
}

export default defineComponent({
  name: "tabScroll",
  setup() {
    return { cells };
  },
});
</script>

<style lang="scss">
.title-divide {
  width: 300px;
  display: block;
  background-color: gray;
  text-align: left;
}
</style>
