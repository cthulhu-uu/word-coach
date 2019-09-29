<template>
  <view class="container">
    <view :style="{ flex: 1, justifyContent: 'center', alignItems: 'center' }">
      <nb-text v-if="isLoaded" :style="{ fontFamily: 'agenda', fontSize: 28 }">
        Hello, world!
      </nb-text>
    </view>
    <text class="title">🎉WORD Coach🎉{{isLoaded}}</text>
    <view class="button">
      <button class="button"
      :on-press="onPressStartGame"
      title="Start gaem"
      color="#841584"
      accessibility-label="Start gaem or go home"
    /></view>
    <nb-view padder>
    <view>
      <nb-button
      :on-press="onPressStartExpert"
      title="hii"
      color="#000"
      ><nb-text>expert mode 😚</nb-text></nb-button>
    </view>
    </nb-view>
    <nb-view padder>
    <view padder>
      <nb-button bordered
      :on-press="handleBtnPress"
      title="hii"
      color="#000"
      ><nb-text>CHamoru?! 🇬🇺</nb-text></nb-button>
    </view>
    </nb-view>
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
    }
  },
  async mounted() {
    await Font.loadAsync({
    'agenda': require('../assets/fonts/agenda.ttf'),
    });

    this.isLoaded = true;
    console.log("Loaded from HomeScreen");

    this.props.fontLoader();

  },
  methods: {
    onPressStartGame: function() {
      this.navigation.navigate("Game", {expert: false});
    },
    onPressStartExpert: function() {
      this.navigation.navigate("Expert", {expert: true});
    },
    handleBtnPress: function() {
      alert('hi honey buns');
    }
  }
};
</script>

<style scoped>
.title {
  font-family: agenda-thin;
  font-size: 56 ;
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