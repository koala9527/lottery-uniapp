<template>
    <view class="container">
      <!-- 第一条帮助信息 -->
      <view class="help-item">
        <view class="question" @tap="toggleAnswer(1)">
          <text>如何使用？</text>
        </view>
        <view v-show="isOpen[1]" class="answer">
          <text>首页点击 “拍照或者香泽选择彩票” 点击确认后系统会自动识别图片，识别过程需要一定的时间（跟图片大小有关），系统处理完成后会将识别的文本返回到结果区域。</text>
        </view>
      </view>
  
      <!-- 第二条帮助信息 -->
      <view class="help-item">
        <view class="question" @tap="toggleAnswer(2)">
          <text>遇到bug怎么办？</text>
        </view>
        <view v-show="isOpen[2]" class="answer">
          <text>如果使用过程中遇到系统bug，请点击意见反馈按钮填写您遇到的问题，我们会在收到反馈后及时处理。</text>
        </view>
      </view>
  
      <!-- 第三条帮助信息 -->
      <view class="help-item">
        <view class="question" @tap="toggleAnswer(3)">
          <text>识别不准怎么办？</text>
        </view>
        <view v-show="isOpen[3]" class="answer">
          <text>如果识别不准确，请注意以下几点：（1.图片是否过长；2.图片是否过于模糊；3.图片是否不够完整）。</text>
        </view>
      </view>

        <!-- 第三条帮助信息 -->
        <view class="help-item">
            <view class="question" @tap="toggleAnswer(4)">
                <text>开奖了怎么还没查询到结果？</text>
            </view>
            <view v-show="isOpen[4]" class="answer">
                <text>由于第三方原因彩票开奖后会延迟半个小时到一个小时。</text>
            </view>
            </view>
    </view>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isOpen: {1: false, 2: false, 3: false, 4: false},
      }
    },
    methods: {
      toggleAnswer(index) {
        this.isOpen[index] = !this.isOpen[index]
      },
    },
  }
  </script>
  
  <style>
  .container {
    padding: 20px;
  }
  
  .help-item {
    margin-top: 10px;
  }
  
  .question {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #f09883;
    box-shadow: 0 0 5px rgba(0,0,0,.1);
    padding: 10px;
    border-radius: 5px;
  }
  
  .answer {
    margin-top: 10px;
    padding: 10px;
    background-color: #fafafa;
    box-shadow: 0 0 5px rgba(0,0,0,.1);
    border-radius: 5px;
  }
  </style>
  