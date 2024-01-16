---
sidebar: false
---

<script setup>
import { withBase } from 'vitepress'
import { ref, reactive } from 'vue';

    function generateLogUrl(dateStr) {
      const baseDir = '/logs/';
      const formattedDate = dateStr.replace(/-/g, '/');
      return withBase(`${baseDir}${formattedDate}`);
    }


// 示例数据结构
const timelineData = reactive({
  2024: {
    1: {
      15: '/logs/2024/01/15',
      20: '/logs/2024/01/20'
    },
    2: {
      10: '/logs/2024/02/10'
    }
  },
  2025: {
    1: {
      5: '/logs/2025/01/05'
    }
  }
});

// 控制年份折叠的状态
const yearsCollapse = ref({});

// 切换年份的折叠状态
const toggleYear = (year) => {
  if (yearsCollapse.value[year] === undefined) {
    yearsCollapse.value[year] = true;
  } else {
    yearsCollapse.value[year] = !yearsCollapse.value[year];
  }
};

// 控制月份折叠的状态
const monthsCollapse = ref({});

// 切换月份的折叠状态
const toggleMonth = (year, month) => {
  const key = `${year}-${month}`;
  if (monthsCollapse.value[key] === undefined) {
    monthsCollapse.value[key] = true;
  } else {
    monthsCollapse.value[key] = !monthsCollapse.value[key];
  }
};
</script>

# 时光轴日志

<!-- 时光轴 -->
 <div class="timeline">
    <ul>
      <li v-for="(months, year) in timelineData" :key="year">
        <div
          class="year"
          :class="{ collapsed: yearsCollapse[year] }"
          @click="toggleYear(year)"
        >
          {{ year }}年
          <span class="toggle-icon" v-if="!yearsCollapse[year]">▼</span>
          <span class="toggle-icon" v-else>▶</span>
        </div>
        <ul v-if="!yearsCollapse[year]">
          <li v-for="(days, month) in months" :key="month">
            <div
              class="month"
              :class="{ collapsed: monthsCollapse[`${year}-${month}`] }"
              @click="toggleMonth(year, month)"
            >
              {{ month }}月
              <span class="toggle-icon" v-if="!monthsCollapse[`${year}-${month}`]">▼</span>
              <span class="toggle-icon" v-else>▶</span>
            </div>
            <ul v-if="!monthsCollapse[`${year}-${month}`]">
              <li v-for="(link, day) in days" :key="day">
                <a :href="generateLogUrl(`${year}-${month}-${day}`)">查看{{`${year}-${month}-${day}`}}工作日志</a>
              </li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </div>

<style module>
.timeline {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #333;
}

.timeline ul {
  list-style-type: none;
  padding-left: 20px;
  margin: 0;
}

.year, .month {
  cursor: pointer;
  font-weight: bold;
  margin-bottom: 5px;
  font-size: 1.2em;
  background-color: #5c6bc0;
  color: white;
  padding: 10px 15px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  transition: background-color 0.3s ease;
}

.year:hover, .month:hover {
  background-color: #7986cb;
}

.collapsed .toggle-icon {
  transform: rotate(90deg);
}

.toggle-icon {
  margin-left: auto;
  font-size: 0.8em;
}

.day {
  background-color: #eeeeee;
  margin: 5px 0;
  padding: 8px 15px;
  border-radius: 10px;
}

a {
  text-decoration: none;
  color: #333;
  display: block;
  transition: background-color 0.3s ease;
}

a:hover {
  background-color: #f0f0f0;
}

/* 这里移除了li元素的左端点样式 */
li {
  list-style: none;
}

/* 新增的末端节点样式 */
.day-end {
  background-color: #cfd8dc;
  margin: 5px 0;
  padding: 8px 15px;
  border-radius: 0;
}

/* 更新展开和隐藏子节点的状态样式 */
.year.expanded, .month.expanded {
  background-color: #3949ab;
}

.year.collapsed, .month.collapsed {
  background-color: #5c6bc0;
}
</style>
