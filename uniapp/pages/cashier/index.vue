<template>
  <view class="cashier">
    <view class="navbar" role="navigation">
      <view class="nav-side nav-left" @tap="goBack" aria-label="返回">
        <text class="icon-left"></text>
      </view>
      <text class="nav-title">收银台</text>
      <view class="nav-side nav-right"></view>
    </view>

    <view class="hero">
      <view class="amount-row">
        <image class="bean" :src="beanIcon" mode="widthFix" />
        <text class="amount">{{ amount.toFixed(2) }}</text>
      </view>

      <view class="hint-card" role="switch" :aria-checked="useBeans" @tap="toggleBeans" aria-label="星豆抵扣开关">
        <text class="hint-text">可用{{ stars }}星抵扣{{ starDeduction }}元</text>
        <view class="switch" :class="{ on: useBeans }" aria-hidden="true">
          <view class="knob"></view>
        </view>
      </view>
    </view>

    <view class="section-title">兑换方式</view>

    <view class="option" :aria-checked="selected==='balance'" @tap="select('balance')" role="radio">
      <view class="radio" :class="{ checked: selected==='balance' }"></view>
      <text class="label">余额兑换</text>
      <view class="meta">
        <text class="link">选择账户</text>
        <text class="chevron small">›</text>
      </view>
    </view>

    <view class="option" :aria-checked="selected==='coupon'" @tap="select('coupon')" role="radio">
      <view class="radio" :class="{ checked: selected==='coupon' }"></view>
      <text class="label">券兑换</text>
      <view class="meta">
        <text class="link">选择券</text>
        <text class="chevron small">›</text>
      </view>
    </view>

    <view class="option disabled" aria-disabled="true" role="radio">
      <view class="radio"></view>
      <text class="label">三方兑换</text>
      <view class="meta">
        <text class="note">不支持核销码订单</text>
      </view>
    </view>

    <view class="placeholder"></view>

    <view class="footer">
      <button class="primary" @tap="confirm">确认兑换</button>
      <view class="home-indicator"></view>
    </view>
  </view>
</template>

<script>
export default {
  name: 'CashierExchange',
  data() {
    return {
      amount: 66,
      stars: 1000,
      starDeduction: 10,
      useBeans: false,
      selected: 'balance'
    }
  },
  computed: {
    beanIcon() {
      return '/assets/CodeBubbyAssets/1714_20101/14.png'
    },
    payable() {
      return Math.max(0, this.amount - (this.useBeans ? this.starDeduction : 0))
    }
  },
  methods: {
    toggleBeans() {
      this.useBeans = !this.useBeans
    },
    select(key) {
      if (key === 'third') return
      this.selected = key
    },
    confirm() {
      uni.showToast({
        title: `提交：${this.selected}，应付¥${this.payable.toFixed(2)}`,
        icon: 'none'
      })
    },
    goBack() {
      if (getCurrentPages().length > 1) {
        uni.navigateBack({ delta: 1 })
      } else {
        uni.switchTab({ url: '/pages/index/index' })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
$bg: #F5F5F5;
$text-333: #333;
$text-666: #666;
$brand: #FF8416;
$accent: #FF5C4D;
$pink: #F3A0A5;

.cashier {
  min-height: 100vh;
  background: $bg;
  padding-bottom: env(safe-area-inset-bottom);
}

.navbar {
  position: sticky;
  top: 0;
  z-index: 10;
  height: 88rpx;
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1rpx solid rgba(0,0,0,0.04);

  .nav-side {
    width: 88rpx;
    height: 88rpx;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .icon-left {
    width: 28rpx;
    height: 28rpx;
    border-left: 6rpx solid $text-333;
    border-bottom: 6rpx solid $text-333;
    transform: rotate(45deg);
  }
  .nav-title {
    flex: 1;
    text-align: center;
    font-size: 36rpx;
    font-weight: 600;
    color: $text-333;
  }
}

.hero {
  padding: 40rpx 24rpx 0;

  .amount-row {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12rpx;

    .bean {
      width: 48rpx;
      height: 48rpx;
    }
    .amount {
      font-size: 64rpx;
      font-weight: 600;
      color: $text-333;
      line-height: 1.2;
    }
  }

  .hint-card {
    margin: 24rpx auto 0;
    width: 702rpx;
    min-height: 72rpx;
    padding: 0 24rpx;
    background: #fff;
    border-radius: 20rpx;
    display: flex;
    align-items: center;
    justify-content: space-between;

    .hint-text {
      font-size: 24rpx;
      color: $text-666;
    }

    .switch {
      width: 88rpx;
      height: 54rpx;
      border-radius: 54rpx;
      background: #EFEFEF;
      position: relative;
      transition: all .2s ease;

      .knob {
        position: absolute;
        top: 4rpx; left: 4rpx;
        width: 46rpx; height: 46rpx;
        background: #fff;
        border-radius: 50%;
        box-shadow: 0 2rpx 6rpx rgba(0,0,0,.08);
        transition: all .2s ease;
      }

      &.on {
        background: #FFBABF;
      }
      &.on .knob {
        transform: translateX(34rpx);
      }
    }
  }
}

.section-title {
  margin: 32rpx 24rpx 8rpx;
  font-size: 26rpx;
  color: $text-333;
}

.option {
  width: 702rpx;
  margin: 16rpx auto 0;
  padding: 22rpx 24rpx;
  background: #fff;
  border-radius: 20rpx;
  display: flex;
  align-items: center;
  gap: 16rpx;

  .radio {
    width: 36rpx;
    height: 36rpx;
    border-radius: 50%;
    border: 4rpx solid #ddd;
    position: relative;

    &.checked {
      border-color: $pink;
    }
    &.checked::after {
      content: '';
      position: absolute;
      left: 50%; top: 50%;
      width: 20rpx; height: 20rpx;
      background: $pink;
      border-radius: 50%;
      transform: translate(-50%, -50%);
    }
  }

  .label {
    font-size: 30rpx;
    color: $text-333;
    flex: 1;
  }

  .meta {
    display: flex;
    align-items: center;
    gap: 8rpx;

    .link {
      font-size: 26rpx;
      color: #999;
    }
    .note {
      font-size: 26rpx;
      color: $accent;
    }
    .chevron {
      font-size: 36rpx;
      color: #999;
      &.small { font-size: 28rpx; }
    }
  }

  &.disabled {
    opacity: .55;
  }
}

.placeholder {
  height: 220rpx; /* 为底部按钮预留滚动空间 */
}

.footer {
  position: fixed;
  left: 0; right: 0; bottom: 0;
  background: transparent;
  padding: 0 30rpx calc(30rpx + env(safe-area-inset-bottom));
  box-sizing: border-box;

  .primary {
    height: 92rpx;
    line-height: 92rpx;
    width: 690rpx;
    margin: 0 auto;
    background: $pink;
    color: #fff;
    font-size: 30rpx;
    font-weight: 600;
    border-radius: 46rpx;
  }

  .home-indicator {
    width: 268rpx;
    height: 10rpx;
    background: #333;
    opacity: .85;
    border-radius: 100rpx;
    margin: 22rpx auto 10rpx;
  }
}
</style>