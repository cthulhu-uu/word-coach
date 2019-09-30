<template>
  <style-wrapper>
    <root>
      <!-- <app-loading v-if="!isLoaded"></app-loading> -->
      <app-navigator v-if="isLoaded"></app-navigator>
    </root>
  </style-wrapper>
   <!-- There used to be codes here 😔 -->

</template>

<script>
import { AppLoading } from "expo";
import Vue from "vue-native-core";
import StyleWrapper from './components/styleWrapper';
import { VueNativeBase, Root } from "native-base";

import {
  createAppContainer,
  createStackNavigator,
} from "vue-native-router";

import HomeScreen from "./screens/HomeScreen.vue";
import GameScreen from "./screens/GameScreen.vue";

const StackNavigator = createStackNavigator(
  {
    Home: HomeScreen,
    Game: GameScreen,
    Expert: {
      screen: GameScreen,
      navigationOptions: {
        expert: true
      }
    }
  },
  {
    initialRouteName: 'Home',
  }
);

const AppNavigator = createAppContainer(StackNavigator);

Vue.use(VueNativeBase);

export default {
  components: { AppNavigator, Root, StyleWrapper, AppLoading },
  data: function() {
    return {
      isLoaded: false
    }
  },
  mounted: async function() {
    await this.loadFonts();
  },
  methods: {
    loadFonts: async function() {
      try {
        await Expo.Font.loadAsync({
          'agenda': require('./assets/fonts/agenda.ttf')
        });
      } catch(error) {
        console.log('some error occurred loading fonts')
        console.log(error);
      }
      this.isLoaded = true;
      console.log("Loaded from App.vue");

    }
  }
}
</script>

<style>
</style>
