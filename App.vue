<template>
    <root>
       <app-navigator></app-navigator>
    </root>
   <!-- There used to be codes here 😔 -->

</template>

<script>
import Vue from "vue-native-core";
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
  },
  {
    initialRouteName: 'Home',
  }
);

const AppNavigator = createAppContainer(StackNavigator);

Vue.use(VueNativeBase);

export default {
  components: { AppNavigator, Root },
}
</script>

<style>

</style>
