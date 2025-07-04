<template>
  <view 
    class="product-card" 
    :class="[`product-card--${layout}`, { 'product-card--shadow': showShadow }]"
    @click="handleClick"
  >
    <!-- 商品图片 -->
    <view class="product-card__image">
      <image 
        :src="product.image" 
        :alt="product.name"
        mode="aspectFill"
        class="product-card__img"
        @error="handleImageError"
      />
      <!-- 角标 -->
      <view v-if="product.tags && product.tags.length" class="product-card__badges">
        <text 
          v-for="tag in product.tags.slice(0, 2)" 
          :key="tag"
          class="product-card__badge"
        >
          {{ tag }}
        </text>
      </view>
      <!-- 收藏按钮 -->
      <view class="product-card__favorite" @click.stop="handleFavorite">
        <uni-icons 
          :type="isFavorited ? 'heart-filled' : 'heart'" 
          :color="isFavorited ? '#ff6b35' : '#999'"
          size="20"
        />
      </view>
    </view>

    <!-- 商品信息 -->
    <view class="product-card__content">
      <view class="product-card__header">
        <text class="product-card__title">{{ product.name }}</text>
        <view v-if="product.rating" class="product-card__rating">
          <uni-rate 
            :value="product.rating" 
            :readonly="true" 
            :size="12"
            color="#ffca3e"
          />
          <text class="product-card__rating-text">({{ product.rating }})</text>
        </view>
      </view>

      <view class="product-card__price">
        <text class="product-card__current-price">¥{{ formatPrice(product.price) }}</text>
        <text 
          v-if="product.originalPrice && product.originalPrice > product.price" 
          class="product-card__original-price"
        >
          ¥{{ formatPrice(product.originalPrice) }}
        </text>
      </view>

      <view v-if="product.sales" class="product-card__sales">
        已售{{ formatSales(product.sales) }}件
      </view>

      <!-- 操作按钮 -->
      <view class="product-card__actions">
        <button 
          class="product-card__btn product-card__btn--cart"
          @click.stop="handleAddCart"
          :disabled="loading"
        >
          <uni-icons type="cart" color="#fff" size="16" />
          <text class="product-card__btn-text">加购物车</text>
        </button>
        <button 
          class="product-card__btn product-card__btn--share"
          @click.stop="handleShare"
        >
          <uni-icons type="redo" color="#666" size="16" />
        </button>
      </view>
    </view>

    <!-- 加载状态 -->
    <view v-if="loading" class="product-card__loading">
      <uni-load-more status="loading" />
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Product {
  id: number
  name: string
  price: number
  originalPrice?: number
  image: string
  rating?: number
  sales?: number
  tags?: string[]
}

interface Props {
  product: Product
  layout?: 'vertical' | 'horizontal'
  showShadow?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  layout: 'vertical',
  showShadow: true
})

const emit = defineEmits<{
  click: [product: Product]
  addCart: [product: Product]
  addFavorite: [product: Product]
  share: [product: Product]
}>()

const loading = ref(false)
const isFavorited = ref(false)

// 格式化价格
const formatPrice = (price: number): string => {
  return price.toFixed(2)
}

// 格式化销量
const formatSales = (sales: number): string => {
  if (sales >= 10000) {
    return `${(sales / 10000).toFixed(1)}万`
  }
  return sales.toString()
}

// 处理点击事件
const handleClick = () => {
  emit('click', props.product)
}

// 处理加购物车
const handleAddCart = async () => {
  try {
    loading.value = true
    // 模拟API调用
    await new Promise(resolve => setTimeout(resolve, 500))
    
    emit('addCart', props.product)
    
    // 显示成功提示
    uni.showToast({
      title: '已加入购物车',
      icon: 'success',
      duration: 1500
    })
  } catch (error) {
    uni.showToast({
      title: '加入失败，请重试',
      icon: 'none'
    })
  } finally {
    loading.value = false
  }
}

// 处理收藏
const handleFavorite = () => {
  isFavorited.value = !isFavorited.value
  emit('addFavorite', props.product)
  
  uni.showToast({
    title: isFavorited.value ? '已收藏' : '已取消收藏',
    icon: 'none',
    duration: 1000
  })
}

// 处理分享
const handleShare = () => {
  emit('share', props.product)
  
  // #ifdef MP-WEIXIN
  uni.showShareMenu({
    withShareTicket: true
  })
  // #endif
  
  // #ifdef H5
  if (navigator.share) {
    navigator.share({
      title: props.product.name,
      text: `发现好商品：${props.product.name}`,
      url: location.href
    })
  } else {
    uni.showToast({
      title: '已复制链接',
      icon: 'none'
    })
  }
  // #endif
}

// 处理图片加载错误
const handleImageError = () => {
  console.error('商品图片加载失败:', props.product.image)
}
</script>

<style lang="scss" scoped>
.product-card {
  background: #fff;
  border-radius: 12rpx;
  overflow: hidden;
  position: relative;
  margin-bottom: 24rpx;
  
  &--shadow {
    box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
  }
  
  &--vertical {
    display: flex;
    flex-direction: column;
  }
  
  &--horizontal {
    display: flex;
    flex-direction: row;
    
    .product-card__image {
      width: 200rpx;
      height: 200rpx;
      flex-shrink: 0;
    }
    
    .product-card__content {
      flex: 1;
      padding: 20rpx;
    }
  }
  
  &__image {
    position: relative;
    width: 100%;
    height: 300rpx;
    background: #f5f5f5;
  }
  
  &__img {
    width: 100%;
    height: 100%;
    display: block;
  }
  
  &__badges {
    position: absolute;
    top: 16rpx;
    left: 16rpx;
    display: flex;
    flex-direction: column;
    gap: 8rpx;
  }
  
  &__badge {
    background: rgba(255, 107, 53, 0.9);
    color: #fff;
    font-size: 20rpx;
    padding: 4rpx 8rpx;
    border-radius: 4rpx;
    line-height: 1;
  }
  
  &__favorite {
    position: absolute;
    top: 16rpx;
    right: 16rpx;
    width: 60rpx;
    height: 60rpx;
    background: rgba(255, 255, 255, 0.9);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(10rpx);
  }
  
  &__content {
    padding: 24rpx;
  }
  
  &__header {
    margin-bottom: 16rpx;
  }
  
  &__title {
    font-size: 28rpx;
    font-weight: 500;
    color: #333;
    line-height: 1.4;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    overflow: hidden;
    margin-bottom: 8rpx;
  }
  
  &__rating {
    display: flex;
    align-items: center;
    gap: 8rpx;
  }
  
  &__rating-text {
    font-size: 22rpx;
    color: #999;
  }
  
  &__price {
    display: flex;
    align-items: baseline;
    gap: 12rpx;
    margin-bottom: 8rpx;
  }
  
  &__current-price {
    font-size: 32rpx;
    font-weight: 600;
    color: #ff6b35;
  }
  
  &__original-price {
    font-size: 24rpx;
    color: #999;
    text-decoration: line-through;
  }
  
  &__sales {
    font-size: 22rpx;
    color: #999;
    margin-bottom: 20rpx;
  }
  
  &__actions {
    display: flex;
    gap: 16rpx;
    align-items: center;
  }
  
  &__btn {
    border: none;
    border-radius: 8rpx;
    font-size: 24rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8rpx;
    transition: all 0.2s;
    
    &--cart {
      flex: 1;
      height: 64rpx;
      background: #ff6b35;
      color: #fff;
      
      &:active {
        background: #e55a2b;
      }
      
      &:disabled {
        background: #ccc;
      }
    }
    
    &--share {
      width: 64rpx;
      height: 64rpx;
      background: #f5f5f5;
      color: #666;
      
      &:active {
        background: #e8e8e8;
      }
    }
  }
  
  &__btn-text {
    font-size: 24rpx;
  }
  
  &__loading {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(255, 255, 255, 0.8);
    display: flex;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(4rpx);
  }
}

// 响应式适配
@media (max-width: 768rpx) {
  .product-card {
    &--horizontal {
      .product-card__image {
        width: 160rpx;
        height: 160rpx;
      }
      
      .product-card__content {
        padding: 16rpx;
      }
    }
  }
}
</style>