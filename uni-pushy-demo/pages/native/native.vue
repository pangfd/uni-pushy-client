<template>
  <view class="page">
    <!-- 信息 -->
    <view>
      <view class="item">
        <view class="label">原生版本：</view> <view class="value">{{ appInfo.nativeVersion }}</view>
      </view>
      <view class="item">
        <text class="label">原生版本号：</text>
        <text class="value">{{ appInfo.nativeVersionCode }}</text>
      </view>

      <view class="item">
        <text class="label">wgt 版本：</text> <text class="value">{{ appInfo.wgtVersion }}</text>
      </view>
      <view class="item">
        <text class="label">wgt 版本号：</text>
        <text class="value">{{ appInfo.wgtVersionCode }}</text>
      </view>
    </view>

    <button @click="onGetInfo" class="btn" style="background-color: #999999;">获取信息</button>
    <button @click="() => onGetUpdate()" class="btn" style="background-color: #dd524d;">检查更新</button>
    <button @click="() => onGetUpdate(true)" class="btn" style="background-color: #dd524d;">检查更新(手动)</button>

    <!-- <view class="info-board" style="color: #007AFF;">
      {{ appInfoString }}
    </view> -->

    <scroll-view class="info-board" scroll-x="true">
      <y-json-view :json="appInfoJson" />
    </scroll-view>
  </view>
</template>

<script>
import { pushy } from '@/utils/pushy/index'
import yJsonView from '@/components/y-json-view/y-json-view'
export default {
  name: 'Native',
  components: {
    yJsonView,
  },
  data() {
    return {
      appInfo: {},
      appInfoJson: {},
    }
  },
  methods: {
    async onGetUpdate(manual) {
      const res = await pushy.getUpdate(manual)
      this.appInfoJson = res
      
      uni.showToast({
        title: res.message,
        icon:'none'
      })
    },

    async onGetInfo() {
      const appInfo = await pushy.getInfo()
      this.appInfo = appInfo
      this.appInfoJson = appInfo
      uni.showToast({
        title: '获取信息成功',
        icon:'none'
      })
    },
  },
  onLoad() {
    this.onGetInfo()
  },
}
</script>

<style lang="scss" scoped>
$padding: 15px;
.page {
  padding: $padding;

  .item {
    box-sizing: border-box;
    height: 44px;
    border-bottom: 1px solid #eee;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .label {
      color: #333;
      font-size: 18px;
      font-weight: bold;
    }
    .value {
      font-size: 16px;
      color: #666;
    }
  }
  .btn {
    margin-top: $padding;
    box-sizing: border-box;

    color: #fff;
  }

  .info-board {
    box-sizing: border-box;
    margin-top: $padding;
    padding: $padding;
    border-radius: 6px;
    border: 1px solid #eee;
  }
}
</style>
