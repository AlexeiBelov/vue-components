<!-- Принимает необязательные props
     options - принимает массив объектов [{ label: 'Все курсы', choice: true }, { label: 'курсы', choice: false }], значение поля choice (true или false) имитирует атрибут selected
     disabled, placeholder.
     Отдает через $emit выбранное значение-->
<template>
  <div class="wrap">
      <input
          id="simpleSelect"
          ref="input"
          type="text"
          readonly
          name="simpleSelect"
          :disabled="disabled"
          :class="['input', 'text', disabled && styleDisabled]"
          :value="value"
          :placeholder="placeholder"
          @click="click"
          @focusout="focusout"
      />
    <div
        v-if="show"
        class="show scrollbar"
        @mouseover="mouseOver"
        @mouseleave="mouseleave">
      <ul
          v-for="(item, i) in list"
          :key="item.label + i"
          @click="clickList"
          :class="item.choice ? styleActive : styleElement">
        <li
            class="text"
            @click="clickItem(item)">
          {{ item.label }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SimpleSelect',
  props: {
    options: {
      type: Array,
      required: false,
      default: () => [{ label: 'Нет данных', choice: false }],
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false,
    },
    placeholder: {
      type: String,
      required: false,
      default: 'Выбрать',
    },
  },
  data() {
    return {
      value: '',
      show: false,
      mouseOverList: false,
    };
  },
  computed: {
    list() {
      return this.options;
    },
    styleElement() {
      return 'element';
    },
    // устанавливает стиль выбранного элемента в выпадающем списке
    styleActive() {
      return 'activeElement';
    },
    // устанавливает стиль при disabled
    styleDisabled() {
      return 'selectDisabled';
    },
  },
  created () {
    this.setChoice();
    this.createOptions();
  },
  methods: {
    // добавляет id
    createOptions() {
      this.options.forEach((item, index) => {
        item.id = item.label + index;
      });
    },
    // отображает\скрывает выпадающий список
    click() {
      this.show = !this.show;
    },
    // потеря фокуса инпутом
    focusout() {
      this.show = this.mouseOverList;
    },
    // курсор над выпадающиим списком
    mouseOver() {
      this.mouseOverList = true;
    },
    // курсор покинул список
    mouseleave() {
      this.mouseOverList = false;
    },
    // клик по списку вне items
    clickList() {
      this.mouseOverList = false;
      this.focusout();
    },
    // устанавливает выбранное значение при загрузке
    setChoice() {
      this.options.forEach((item) => {
        if (item.choice) {
          this.value = item.label;
        }
      })
    },
    // обрабатывает выбор в выпадающем списке, меняет стиль выбранного элемента и генерирует всплытие выбранного значения в родительский элемент
    clickItem(event) {
      this.value = event.label;
      this.options.forEach((item) => {
        item.choice = item.id === event.id
      });
    },
  },
};
</script>

<style scoped>
.wrap {
  width: 240px;
}
.activeElement {
  box-sizing: border-box;
  display: flex;
  flex-basis: auto;
  flex-direction: row;
  align-items: center;
  max-width: 232px;
  height: 32px;
  padding: 2px 12px 8px;
  margin: 8px 6px;
  cursor: pointer;
  color: #409eff;;
}
.selectDisabled {
  background: #e7e9ed;
  cursor: default;
}
.input {
  -webkit-appearance: none;
  border-radius: 4px;
  border: 1px solid #dcdfe6;
  box-sizing: border-box;
  color: #606266;
  display: inline-block;
  font-size: inherit;
  height: 40px;
  line-height: 40px;
  outline: none;
  padding: 0 24px 0 8px;
  transition: border-color .2s cubic-bezier(.645,.045,.355,1);
  position: relative;
  overflow: hidden;
  background: #fff url("./icon/arrows_down.png") no-repeat 214px 12px;
  width: 100%;
  cursor: pointer;
}
.show {
  position: relative;
  z-index: 100;
  width: 240px;
  max-height: 274px;
  margin: 4px 0 0 0;
  border: 1px solid #e4e7ed;
  border-radius: 4px;
  background-color: #fff;
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%);
  box-sizing: border-box;
  overflow-y: auto;
  -webkit-transition: max-height .5s linear;
  -moz-transition: max-height .5s linear;
  transition: max-height .5s linear;
}
.show span{
  max-width: 240px;
}
.show img {
  padding: 2px 0 0 8px;
}
.element {
  box-sizing: border-box;
  display: flex;
  flex-basis: auto;
  flex-direction: row;
  align-items: center;
  max-width: 240px;
  height: 32px;
  padding: 2px 12px 8px;
  margin: 8px 4px;
  background: #fff;
  cursor: pointer;
}
.text {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.scrollbar::-webkit-scrollbar {
  width: 12px;               /* ширина scrollbar */
}
.scrollbar::-webkit-scrollbar-track {
  background: #e9e9eb;        /* цвет дорожки */
}
.scrollbar::-webkit-scrollbar-thumb {
  background-color: #909399;    /* цвет плашки */
  border-radius: 24px;       /* закругления плашки */
  border: 1px solid #f4f4f5;  /* padding вокруг плашки */
}
</style>