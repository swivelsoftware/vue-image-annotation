<template>
  <draggable class="imgdw-toolbar" ref="draggable">
    <brush :curBrush="curBrush" @onSelectBrush="handleSelectBrush" />
    <div class="imgdw-split-line" />
    <v-icon class="pa-1" @click.native.stop="handleUndo">undo</v-icon>
    <v-icon class="pa-1" @click.native.stop="handleScale('narrow')">zoom_out</v-icon>
    <v-icon class="pa-1" @click.native.stop="handleScale('enlarge')">zoom_in</v-icon>
    <v-icon color="red" class="pa-1" @click.native.stop="handleCancel">cancel</v-icon>
    <v-icon color="green" class="pa-1" @click.native.stop="handleConfirm">check_circle</v-icon>
  </draggable>
</template>

<script>
import draggable from '../draggable/draggable.vue';
import brush from '../brush/brush.vue';

export default {
  name: 'imgdwToolbar',

  components: {
    draggable,
    brush
  },

  props: {
    canvasScale: Number,
    curBrush: String
  },

  methods: {
    initPosition() {
      this.$refs.draggable.initPosition();
    },

    handleSelectBrush(brush) {
      this.$emit('onSelectBrush', brush);
    },

    handleUndo() {
      this.$emit('onUndo');
    },

    handleScale(type) {
      if (type === 'enlarge' && this.canvasScale < 2) {
        this.$emit('onScale', parseFloat((this.canvasScale + 0.1).toFixed(1)));
      }

      if (type === 'narrow' && this.canvasScale > 0.1) {
        this.$emit('onScale', parseFloat((this.canvasScale - 0.1).toFixed(1)));
      }
    },

    handleCancel() {
      this.$emit('onCancel');
    },

    handleConfirm() {
      this.$emit('onConfirm');
    }
  }
};
</script>

<style lang="less" scoped>
.imgdw-toolbar {
  position: absolute;
  height: 42px;
  background-color: rgb(255, 255, 255);
  display: flex;
  align-items: center;
  padding: 0 15px;
  border-radius: 2px;
  box-shadow: 0 0 3px 0 #333;

  * {
    user-select: none;
  }
}

.imgdw-btn {
  font-size: 26px;
  cursor: pointer;
  color: #fff;
}

.imgdw-split-line {
  height: 24px;
  border-right: 1px solid #ddd;
  margin-right: 20px;
}

.imgdw-scale-percent {
  width: 60px;
}

.imgdw-btn-undo {
  margin-right: 20px;
}

.imgdw-btn-narrow {
  margin-right: 10px;
}

.imgdw-btn-cancel {
  margin-left: 20px;
  color: rgb(250, 58, 44);
  margin-right: 10px;
}

.imgdw-btn-confirm {
  color: #2bc96a;
}
</style>
