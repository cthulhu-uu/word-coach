<template>
  <view class="container">
    <text :class="screenWidth < 375 ? 'smaller-title' : 'title'"
      >W O R D   C O A C H</text>
    <view>
      <nb-button bordered
      :on-press="onPressStartGame"
      title="Start gaem"
      color="#841584"
      accessibility-label="Start gaem or go home"
      ><nb-text>s t a r t</nb-text></nb-button>
    </view>
    <nb-view padder>
    <view>
      <nb-button
      :on-press="onPressStartExpert"
      title="hii"
      color="#000"
      ><nb-text>e x p e r t</nb-text></nb-button>
    </view>
    </nb-view>
    <nb-view padder>
    <!-- <view padder>
      <nb-button bordered
      :on-press="handleBtnPress"
      title="hii"
      color="#000"
      ><nb-text>CHamoru?! 🇬🇺</nb-text></nb-button>
    </view> -->
    </nb-view>
  </view>
</template>

<script>
import { Dimensions } from "react-native";

export default {
  name: "HomeScreen",
  props: {
    navigation: {
      type: Object
    }
  },

  data: function() {
    return {
      screenWidth: Dimensions.get("window").width,
    }
  },

  methods: {
    onPressStartGame: function() {
      this.navigation.navigate("Game", {expert: false});
    },
    onPressStartExpert: function() {
      this.navigation.navigate("Expert", {expert: true});
    },
    handleBtnPress: function() {
      alert('C O M I N G  S O O N');
    }
  }
};
</script>

<style scoped>
.title {
  font-family: agenda;
  font-size: 42;
  margin-bottom: 128;
}
.smaller-title {
  font-family: agenda;
  font-size: 36;
  margin-bottom: 128;

}
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