<template>
  <view class="navbar" :class="[`navbar--${type}`, { 'navbar--fixed': fixed }]" :style="navbarStyle">
    <!-- 状态栏占位 -->
    <view class="navbar__status-bar" :style="{ height: statusBarHeight + 'px' }"></view>
    
    <!-- 导航栏内容 -->
    <view class="navbar__content" :style="{ height: navBarHeight + 'px' }">
      <!-- 左侧区域 -->
      <view class="navbar__left" @click="handleLeftClick">
        <slot name="left">
          <view v-if="showBack" class="navbar__back">
            <uni-icons type="left" :color="iconColor" size="20" />
            <text v-if="backText" class="navbar__back-text" :style="{ color: textColor }">
              {{ backText }}
            </text>
          </view>
        </slot>
      </view>
      
      <!-- 中间标题区域 -->
      <view class="navbar__center">
        <slot name="center">
          <text v-if="title" class="navbar__title" :style="{ color: textColor }">
            {{ title }}
          </text>
        </slot>
      </view>
      
      <!-- 右侧区域 -->
      <view class="navbar__right" @click="handleRightClick">
        <slot name="right">
          <view v-if="rightText" class="navbar__right-text" :style="{ color: textColor }">
            {{ rightText }}
          </view>
          <uni-icons 
            v-else-if="rightIcon" 
            :type="rightIcon" 
            :color="iconColor" 
            size="20"
          />
        </slot>
      </view>
    </view>
    
    <!-- 搜索栏 -->
    <view v-if="showSearch" class="navbar__search" :style="{ paddingBottom: searchPadding + 'rpx' }">
      <view class="navbar__search-box">
        <uni-icons type="search" color="#999" size="16" />
        <input 
          v-model="searchValue"
          class="navbar__search-input"
          :placeholder="searchPlaceholder"
          :placeholder-style="searchPlaceholderStyle"
          @input="handleSearchInput"
          @confirm="handleSearchConfirm"
          @focus="handleSearchFocus"
          @blur="handleSearchBlur"
        />
        <view v-if="searchValue" class="navbar__search-clear" @click="clearSearch">
          <uni-icons type="clear" color="#999" size="14" />
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

interface Props {
  // 基础属性
  title?: string
  type?: 'default' | 'transparent' | 'gradient'
  fixed?: boolean
  
  // 背景和颜色
  backgroundColor?: string
  textColor?: string
  iconColor?: string
  
  // 左侧配置
  showBack?: boolean
  backText?: string
  
  // 右侧配置
  rightText?: string
  rightIcon?: string
  
  // 搜索功能
  showSearch?: boolean
  searchPlaceholder?: string
  searchValue?: string
  
  // 样式配置
  borderBottom?: boolean
  shadow?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  type: 'default',
  fixed: true,
  backgroundColor: '#ffffff',
  textColor: '#333333',
  iconColor: '#333333',
  showBack: true,
  searchPlaceholder: '请输入搜索内容',
  searchValue: '',
  borderBottom: true,
  shadow: false
})

const emit = defineEmits<{
  leftClick: []
  rightClick: []
  searchInput: [value: string]
  searchConfirm: [value: string]
  searchFocus: []
  searchBlur: []
}>()

// 响应式数据
const searchValue = ref(props.searchValue)
const statusBarHeight = ref(0)
const navBarHeight = ref(44)
const searchPadding = ref(24)

// 系统信息
onMounted(() => {
  const systemInfo = uni.getSystemInfoSync()
  statusBarHeight.value = systemInfo.statusBarHeight || 0
  
  // #ifdef MP-WEIXIN
  const menuButtonInfo = uni.getMenuButtonBoundingClientRect()
  navBarHeight.value = menuButtonInfo.height + (menuButtonInfo.top - statusBarHeight.value) * 2
  // #endif
  
  // #ifdef H5
  navBarHeight.value = 44
  // #endif
  
  // #ifdef APP-PLUS
  navBarHeight.value = 44
  // #endif
})

// 计算样式
const navbarStyle = computed(() => {
  const style: Record<string, any> = {}
  
  if (props.type === 'default') {
    style.backgroundColor = props.backgroundColor
    
    if (props.borderBottom) {
      style.borderBottom = '1rpx solid #ebedf0'
    }
    
    if (props.shadow) {
      style.boxShadow = '0 2rpx 8rpx rgba(0, 0, 0, 0.1)'
    }
  } else if (props.type === 'transparent') {
    style.backgroundColor = 'transparent'
  } else if (props.type === 'gradient') {
    style.background = 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)'
  }
  
  return style
})

const searchPlaceholderStyle = computed(() => {
  return 'color: #999; font-size: 28rpx;'
})

// 事件处理
const handleLeftClick = () => {
  if (props.showBack) {
    // 默认返回行为
    const pages = getCurrentPages()
    if (pages.length > 1) {
      uni.navigateBack()
    } else {
      uni.switchTab({
        url: '/pages/index/index'
      })
    }
  }
  emit('leftClick')
}

const handleRightClick = () => {
  emit('rightClick')
}

const handleSearchInput = (e: any) => {
  searchValue.value = e.detail.value
  emit('searchInput', e.detail.value)
}

const handleSearchConfirm = (e: any) => {
  emit('searchConfirm', e.detail.value)
}

const handleSearchFocus = () => {
  emit('searchFocus')
}

const handleSearchBlur = () => {
  emit('searchBlur')
}

const clearSearch = () => {
  searchValue.value = ''
  emit('searchInput', '')
}

// 导出方法
defineExpose({
  clearSearch
})
</script>

<style lang="scss" scoped>
.navbar {
  position: relative;
  background: #fff;
  z-index: 999;
  
  &--fixed {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
  }
  
  &--transparent {
    background: transparent !important;
    
    .navbar__content {
      background: transparent;
    }
  }
  
  &--gradient {
    .navbar__title,
    .navbar__back-text,
    .navbar__right-text {
      color: #fff !important;
    }
  }
  
  &__status-bar {
    width: 100%;
    background: inherit;
  }
  
  &__content {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 32rpx;
    background: inherit;
    
    // #ifdef MP-WEIXIN
    padding-right: 190rpx; // 为胶囊按钮预留空间
    // #endif
  }
  
  &__left {
    display: flex;
    align-items: center;
    min-width: 120rpx;
    justify-content: flex-start;
  }
  
  &__back {
    display: flex;
    align-items: center;
    gap: 8rpx;
    padding: 8rpx;
    margin: -8rpx;
  }
  
  &__back-text {
    font-size: 28rpx;
  }
  
  &__center {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0 32rpx;
  }
  
  &__title {
    font-size: 32rpx;
    font-weight: 600;
    text-align: center;
    max-width: 400rpx;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  
  &__right {
    display: flex;
    align-items: center;
    min-width: 120rpx;
    justify-content: flex-end;
  }
  
  &__right-text {
    font-size: 28rpx;
    padding: 8rpx;
    margin: -8rpx;
  }
  
  &__search {
    padding: 0 32rpx;
    background: inherit;
  }
  
  &__search-box {
    display: flex;
    align-items: center;
    background: #f5f5f5;
    border-radius: 24rpx;
    padding: 16rpx 24rpx;
    gap: 16rpx;
  }
  
  &__search-input {
    flex: 1;
    font-size: 28rpx;
    color: #333;
    background: transparent;
    border: none;
    outline: none;
    
    &::placeholder {
      color: #999;
    }
  }
  
  &__search-clear {
    padding: 8rpx;
    margin: -8rpx;
  }
}

// 平台适配
// #ifdef H5
.navbar {
  &__content {
    padding-right: 32rpx;
  }
}
// #endif

// #ifdef APP-PLUS
.navbar {
  &__content {
    padding-right: 32rpx;
  }
}
// #endif

// 暗黑模式适配
@media (prefers-color-scheme: dark) {
  .navbar {
    &--default {
      background: #1a1a1a;
      border-bottom-color: #333;
    }
    
    &__title,
    &__back-text,
    &__right-text {
      color: #fff !important;
    }
    
    &__search-box {
      background: #333;
    }
    
    &__search-input {
      color: #fff;
      
      &::placeholder {
        color: #666;
      }
    }
  }
}
</style>