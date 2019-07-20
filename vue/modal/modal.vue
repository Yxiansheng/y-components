<template>
  <transition name="modal" tag="div">
    <div class="y-modal" v-show="visible" transition="fade">
      <div class="y-modal-dialog" :style="{width, 'min-width': minWidth}">
        <div class="y-modal-content">
          <!--头部-->
          <div class="y-modal-header">
            <slot name="header">
              <p class="title">{{title}}</p>
            </slot>
            <img @click="cancelHandler()" id="icon_close" src="./icons/guanbi.svg" />
          </div>

          <!--内容区域-->
          <div class="y-modal-body" :style="{height, 'min-height': minHeight}">
            <slot></slot>
          </div>

          <!--尾部,操作按钮-->
          <div class="y-modal-footer" v-if="modalFooterVisible">
            <slot name="footer">
              <slot name="button">
                <a
                  v-if="showCancelButton"
                  href="javascript:void(0)"
                  class="button"
                  :class="cancelButtonClass"
                  @click="cancelHandler"
                >{{cancelButtonText}}</a>
                <a
                  v-if="showConfirmButton"
                  href="javascript:void(0)"
                  class="button"
                  :class="confirmButtonClass"
                  @click="confirmHandler"
                >{{confirmButtonText}}</a>
              </slot>
            </slot>
          </div>
        </div>
      </div>
    </div>
    <!-- <div v-show="show" class="modal-backup"></div> -->
  </transition>
</template>

<script>
export default {
  data() {
    return {
      show: this.visible
    };
  },

  props: {
    visible: { type: Boolean, default: false }, // 模态框是否可见
    modalFooterVisible: { type: Boolean, default: true }, // 模态框底部是否可见
    title: { default: "提示" }, // 模态框标题
    showCancelButton: { default: true }, // 是否显示取消按钮
    cancelButtonClass: { default: "btn-default" }, // 取消按钮样式
    cancelButtonText: { default: "取消" }, // 取消按钮文字
    showConfirmButton: { default: true }, // 是否显示确定按钮
    confirmButtonClass: { default: "btn-active" }, // 确定按钮样式
    confirmButtonText: { default: "确定" }, // 确定按钮文字
    width: { default: "400px" }, // 模态框宽度
    minWidth: { default: "150px" }, // 模态框最小宽度
    height: { default: "auto" }, // 模态框内容盒子高度
    minHeight: { default: "25px" } // 模态框最小高度
  },

  methods: {
    confirmHandler() {
      this.$emit("confirm"); // 传递确认事件
    },
    cancelHandler() {
      this.$emit("update:visible", false); // 更新visible的值
      this.$emit("cancel"); // 传递取消事件
    }
  }
};
</script>

<style scoped>
.modal-enter-active {
  animation: modal-in 0.18s linear;
}

.modal-leave-active {
  animation: modal-in 0.18s reverse linear;
}

@keyframes modal-in {
  0% {
    transform: translateY(-20px) rotateX(-35deg);
    opacity: 0;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    transform: translateY(0) rotateX(0);
    opacity: 1;
  }
}

.y-modal {
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  z-index: 98;
  outline: 0;
  overflow: hidden;
  background-color: rgba(0, 0, 0, 0.5);
}

.y-modal-dialog {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: auto;
  width: 4.8rem;
  background: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  border-radius: 5px;
  box-sizing: border-box;
}
.y-modal-header {
  height: 45px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f5f5f5;
  border-radius: 5px;
  padding: 0 3%;
}
.y-modal-header .title {
  font-size: 15px;
  font-weight: 700;
  margin: 0;
  color: #333;
}
.y-modal-header a {
  color: #999;
}
.y-modal-header a:hover {
  color: #666;
}

.y-modal-body {
  padding: 20px 3%;
}

.y-modal-footer {
  text-align: right;
  padding-top: 10px;
  padding-bottom: 10px;
  border-top: 1px solid #eee;
}
.y-modal-footer a {
  display: inline-block;
  color: #fff;
  padding: 2% 5%;
  border-radius: 5px;
}
.y-modal-footer a:first-of-type {
  background-color: orange;
  margin-right: 15px;
}
.y-modal-footer a:last-of-type {
  background-color: #269;
}

.y-modal-backup {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1000;
  background: rgba(0, 0, 0, 0.5);
}

#icon_close {
  width: 16px;
  cursor: pointer;
}
</style>

