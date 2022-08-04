<template>
  <el-form
    class="form-info"
    ref="formInfo"
    :model="formInfo"
    :label-width="labelWidth"
    :formRules="formRules"
    :hide-required-asterisk="isStatus"
  >
    <template>
      <template v-for="(item, index) in formLists">
        <template v-if="!item.__slotName">
          <el-form-item
            :required="item.isRequire"
            :key="index"
            :label="item.label"
            :prop="item.prop"
          >
            <el-input
              v-if="item.formItemType == 'input'"
              v-model="formInfo[item.prop]"
              :type="item.inputType || 'text'"
              :disabled="item.disabled"
              :clearable="true"
              :maxlength="item.maxlength"
              :placeholder="item.placeholder"
              show-word-limit
            ></el-input>
            <el-select
              :clearable="true"
              v-else-if="item.formItemType == 'select'"
              v-model="formInfo[item.prop]"
              :disabled="item.disabled"
              :placeholder="item.placeholder"
            >
              <el-option
                v-for="(optItem, index) in item.optList"
                :key="index"
                :label="optItem.label"
                :value="optItem.label"
              ></el-option>
            </el-select>
            <el-date-picker
              :value-format="item.dateFormate"
              v-else-if="item.formItemType == 'date'"
              v-model="formInfo[item.prop]"
              :type="item.formItemType"
              :disabled="item.disabled"
              :clearable="true"
              :placeholder="item.label"
            ></el-date-picker>
          </el-form-item>
        </template>
        <template v-if="item.__slotName">
          <slot :name="item.__slotName"></slot>
        </template>
      </template>
    </template>
  </el-form>
</template>
<script>
/*
formLists:[
  {
    type:' formItemType === input 时的类型  例如：textarea '
    formItemType:'渲染的类型 input/select/date',
    disabled:'是否必填项',
    placeholder:'默认的提示信息',
    label:'标题',
    prop:'字段', --- 与 formInfo 中的 key 值对应
  }
]
*/
export default {
  components: {},
  props: {
    formLists: {
      type: Array,
      required: true
    },
    formInfo: {
      type: Object,
      required: true
    },
    // 验证规则
    formRules: {
      type: Object
    },
    // 标题宽度
    labelWidth: {
      type: String,
      default: '5rem'
    }
  },
  data() {
    return {
    };
  },
  mounted() {
    this.setDefaultFormInfo();
  },
  methods: {
    // 设置表单默认值
    setDefaultFormInfo() {
      const formData = { ...this.formInfo };
      this.formLists.forEach(({ key, value }) => {
        if (formData[key] === undefined || formData[key] === null) {
          formData[key] = value;
        }
      });
    },
    // 提交表单
    submitForm() {
      this.$refs['formInfo'].validate([], (valid) => {
        if (valid) {
          this.$emit('submitForm', valid);
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    // 重置表单
    resetForm() {
      this.$refs['formInfo'].resetFields();
    }
  }
};
</script>

<style>
</style>
