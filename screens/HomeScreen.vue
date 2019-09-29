<template>
  <view class="container">
    <button v-bind:title="message" v-bind:on-press="handleBtnPress"/>


    <text class="text-color-primary">{{message}}</text>
          <image
          v-for="i in [1,2,3,4]"
          :style="{width: 200 + i*20, height: 50 + i*10}"
          :source="{uri: 'https://media3.giphy.com/media/yYo9Y9qUJFa1i/giphy.gif'}"
        />
    <view class="button">
      <button class="button"
      :on-press="onPressStartGame"
      title="Start gaem"
      color="#841584"
      accessibility-label="Start gaem or go home"
    /></view>
    <view>
      <nb-button
      :on-press="handleBtnPress"
      title="hii"
      color="#000"
      ><nb-text>hi! 😚</nb-text></nb-button>
    </view>
  </view>
</template>

<script>
export default {
  name: "HomeScreen",
  props: {
    navigation: {
      type: Object
    }
  },
  data: function() {
    return {
      message: "🎉WORD Coach🎉",
      text: ''
    };
  },
  methods: {
    onPressStartGame: function() {
        this.navigation.navigate("Game");
    },
    handleBtnPress: function() {
      alert(this.text + ' honey buns');
    }
  }
};
</script>

<style scoped>
.container {
  flex: 1;
  background-color: white;
  align-items: center;
  justify-content: center;
}
.text-color-primary {
  color: blue;
  font-size: 40;
}
.button {
  background-color: #fff;
  color: black;
  padding: 10px 20px;
  text-align: center;
  display: flex;
  margin: 4px 2px;
  border-radius: 16px;
  border-color: aqua;
  border-width: 2px;
  
}
</style>