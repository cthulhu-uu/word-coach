<template>
    <nb-container>
        <nb-content padder>
            <nb-card>
                <nb-card-item>
                    <nb-text>Which is similar to Honey Buns?</nb-text>
                </nb-card-item>
                <nb-card-item button :on-press="correct">
                    <nb-body>
                        <nb-text>Cleamy</nb-text>
                    </nb-body>
                </nb-card-item>
                <nb-card-item button :on-press="wrong">
                    <nb-body>
                        <nb-text>Selted Egg Chips</nb-text>
                    </nb-body>
                </nb-card-item>
            </nb-card>
        </nb-content>
        <!-- <nb-button
            :on-press="goHome"
            title="go home 😔"
        ><nb-text>go home 😔</nb-text>
        </nb-button>  -->
    </nb-container>
</template>

<script>
import React from "react";
import { Toast } from "native-base";
export default {
    name: "GameScreen",
    props: {
    navigation: {
      type: Object
    }
  },
    methods: {
        goHome() {
            this.navigation.navigate("Home");
        },
        correct() {
            // alert('so cleam 🥳');
            Toast.show({
                text: "so cleam 🥳",
                buttonText: "Okay",
                type: "success"
            });
        },
        wrong() {
            // alert('so wrong 💀');
            Toast.show({
                text: "so wrong 💀",
                buttonText: "Awww",
                type: "danger"
            });
        }
    }
}
</script>

<style scoped>


</style>