<template>
  <t-space direction="vertical">
    <!-- 方式一：使用 options 自定义下拉选项内容 -->
    <t-cascader
      v-model="value1"
      :popup-props="{ overlayClassName: 'tdesign-demo-select__overlay-option' }"
      :options="optionsData"
      clearable
    />
    <!-- 方式二：使用插槽自定义下拉选项内容 -->
    <t-cascader
      v-model="value2"
      :popup-props="{ overlayClassName: 'tdesign-demo-select__overlay-option' }"
      :options="options"
    >
      <template #option="{ item }">
        <div class="tdesign-demo__user-option">
          <img src="https://tdesign.gtimg.com/site/avatar.jpg" />
          <div class="tdesign-demo__user-option-info">
            <div>{{ item.label }}</div>
            <div>{{ item.value }}</div>
          </div>
        </div>
      </template>
    </t-cascader>
    <!-- 方式三：使用option传参自定义下拉选项内容 -->
    <t-cascader
      v-model="value3"
      :popup-props="{ overlayClassName: 'tdesign-demo-select__overlay-option' }"
      :options="options"
      :option="optionRender"
    >
    </t-cascader>
  </t-space>
</template>
<script setup lang="jsx">
import { ref, computed } from 'vue';

const options = [
  {
    label: '选项一',
    value: '1',
    children: [
      {
        label: '子选项一',
        value: '1.1',
      },
      {
        label: '子选项二',
        value: '1.2',
      },
      {
        label: '子选项三',
        value: '1.3',
      },
    ],
  },
  {
    label: '选项二',
    value: '2',
    children: [
      {
        label: '子选项一',
        value: '2.1',
      },
      {
        label: '子选项二',
        value: '2.2',
      },
    ],
  },
];

const value1 = ref('');
const value2 = ref('');
const value3 = ref('');

const optionRender = (h, { item }) => (
  <div class="tdesign-demo__user-option">
    <img src="https://tdesign.gtimg.com/site/avatar.jpg" />
    <div class="tdesign-demo__user-option-info">
      <div>{item.label}</div>
      <div>{item.value}</div>
    </div>
  </div>
);

const getDeepOptions = (options) => {
  if (!options) return null;
  return options.map((item) => ({
    ...item,
    children: getDeepOptions(item.children),
    // content 自定义下拉选项关键代码
    content: (h) => optionRender(h, { item }),
  }));
};

const optionsData = computed(() => getDeepOptions(options));
</script>
<style>
.tdesign-demo__user-option {
  display: flex;
}

.tdesign-demo__user-option > img {
  max-width: 40px;
  max-height: 40px;
  border-radius: 50%;
}

.tdesign-demo__user-option-desc {
  font-size: 14px;
  color: var(--td-text-color-secondary);
}

.tdesign-demo__user-option-info {
  margin-left: 16px;
}

.tdesign-demo-select__overlay-option .t-cascader__item {
  height: auto;
  padding: 8px;
}
</style>
