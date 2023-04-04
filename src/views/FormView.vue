<template>

    <ion-modal :is-open="openModal">
        <ion-header>
        <ion-toolbar>
            <ion-title>Modal</ion-title>
            <ion-buttons slot="end">
            <ion-button @click="changeState">Close</ion-button>
            </ion-buttons>
        </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
        <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Magni illum quidem recusandae ducimus quos
            reprehenderit. Veniam, molestias quos, dolorum consequuntur nisi deserunt omnis id illo sit cum qui. Eaque,
            dicta.
        </p>
        </ion-content>
    </ion-modal>
  </template>
  
  <script setup lang="ts">
  import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButton, IonButtons, IonIcon, IonCardHeader, IonText, IonList, IonItemSliding, IonItem, IonLabel, IonItemOptions, IonItemOption, IonModal } from '@ionic/vue';
  </script>
  
  
  <script lang="ts">
  
    export default {
    
        props: {
            isOpen: Boolean
        },

        data() {
            return {
            openModal: false
            }
        },

        methods: {
            changeState() {
                this.isOpen = !this.isOpen
                console.log(this.isOpen)
            }
        }
    }
  
  </script>