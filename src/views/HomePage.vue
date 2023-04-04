<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Entries</ion-title>

        <ion-buttons slot="end">
          <ion-button @click="changeState">
            <ion-icon :icon="addOutline"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">

      

      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Entries</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-list :inset="true">
        <ion-item-sliding color="medium">
          <ion-item>
            <ion-label>
              <h1 style="font-weight: bold">Match 18, Team 8592</h1>
              <p>Scouter Name: Aryan Tadepalli</p>
            </ion-label>
          </ion-item>

          <ion-item-options>
            <ion-item-option>Favorite</ion-item-option>
            <ion-item-option color="danger">Delete</ion-item-option>
          </ion-item-options>
        </ion-item-sliding>

        

       
      </ion-list>




      <ion-modal :is-open="openModal">
        <ion-header>
          <ion-toolbar>
            <ion-title>Match Data</ion-title>
            <ion-buttons slot="end">
              <ion-button @click="changeState" class="font-bold">Add</ion-button>
            </ion-buttons>
            <ion-buttons slot="start">
              <ion-button @click="changeState">Close</ion-button>
            </ion-buttons>
          </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding modal-content">
    
          

          <div class="pb-24">
            <ion-list :inset="true">
            <ion-item-divider>
              Basic Data
            </ion-item-divider>
            <ion-item>
              <ion-input  v-model="name" placeholder="Scouter Name"/>
            </ion-item>
            <ion-item>
              <ion-input inputmode="numeric" v-model="tnumber" placeholder="Team Number"/>
            </ion-item>
            <ion-item>
              <ion-input inputmode="numeric" v-model="mnumber" placeholder="Match Number"/>
            </ion-item>
          </ion-list>
        
          
          <ion-list :inset="true">
            <ion-item-divider>
              Autonomous
            </ion-item-divider>
            <ion-item>
              <div class="w-full flex items-center justify-between">
                <p>Start Piece</p>
                <select v-model="startpiece" class="text-neutral-500 outline-none w-28">
                  <option value="0">None</option>
                  <option value="1">Cone</option>
                  <option value="2">Cube</option>
         
                </select>
                
              </div>
            </ion-item>


            <ion-item>
              <ion-checkbox color="success" v-model="leftcomm">Did leave community?</ion-checkbox>
            </ion-item>

            <ion-item class="clickable" @click="openModal3 = true">
              <p>Start Position</p>
            </ion-item>

            <ion-item class="clickable" @click="{openModal2 = true; mode = false}">
              <p>Cubes and Cones</p>
            </ion-item>

            
            <ion-item>
              <div class="w-full flex items-center justify-between">
                <p>Charge Station Status</p>
                <select v-model="autocs" class="text-neutral-500 outline-none w-28 whitespace-pre-wrap my-3">
                  <option value="0">None</option>
                  <option value="1">Docked</option>
                  <option value="2">Docked + Engaged</option>
         
                </select>
                
              </div>
            </ion-item>
          </ion-list>

          <ion-list :inset="true">
            <ion-item-divider>
              TeleOp
            </ion-item-divider>
            

            <ion-item class="clickable" @click="{openModal2 = true; mode = true}">
              <p>Cubes and Cones</p>
            </ion-item>

            <ion-item>
              <div class="w-full flex items-center justify-between">
                <p>Charge Station Status</p>
                <select v-model="telecs" class="text-neutral-500 outline-none w-28 whitespace-pre-wrap my-3">
                  <option value="0">None</option>
                  <option value="1">Docked</option>
                  <option value="2">Docked + Engaged</option>
         
                </select>
                
              </div>
            </ion-item>
          </ion-list>


          <ion-list :inset="true" class="mb-24">
            <ion-item-divider>
              Ratings and Comments
            </ion-item-divider>
            

            <ion-item>
              <ion-textarea v-model="pros" class="my-1" label="" placeholder="What did you like about their performance?"></ion-textarea>
            </ion-item>

            <ion-item>
              <ion-textarea v-model="cons" class="my-1" label="" placeholder="What did you dislike about their performance?"></ion-textarea>
            </ion-item>

            <ion-item>
              <ion-textarea v-model="other" class="my-1" label="" placeholder="Other comments"></ion-textarea>
            </ion-item>

            
          </ion-list>
          </div>
        </ion-content>
        


        
      </ion-modal>

      <ion-modal :isOpen="openModal2">

        <ion-header>
          <ion-toolbar>
            <ion-title>Cubes and Cones</ion-title>
            <ion-buttons slot="end">
              <ion-button @click="openModal2 = false" class="font-bold">Close</ion-button>
            </ion-buttons>

          </ion-toolbar>
        </ion-header>
            
        <ion-content class="ion-padding modal-content flex justify-center items-center bg-red-100">
          
          <div class="flex flex-col h-5/6 justify-center align-center [&>*]:my-4 text-center">
        
            <ion-label>
              <h1 style="font-weight: bold; font-size: 4em">{{mode ? telePts : autoPts}}</h1>
              <h1 style="font-weight: bold;">{{mode ? "TeleOp" : "Autonomous"}} Points Scored</h1>
            </ion-label>
              
            
            <div class="flex [&>*]:mx-1 justify-evenly">
              <ion-text color="success" class="text-center font-bold text-xl" style="writing-mode: vertical-rl; text-orientation: mixed;">Top</ion-text>
              <div class="flex items-center"> 
                <ion-button fill="clear" color="danger" size="small" @click="pointManager(mode, 0, mode ? 5 : 6, false, 0)"><ion-icon :icon="removeCircle"></ion-icon></ion-button>
                <div class="zstack w-12">
                  <img src="../images/cone.png">
                  <p>{{ mode ? teleCubesCones[0][0] : autoCubesCones[0][0] }}</p>
                </div>

                <ion-button fill="clear" color="success" size="small" @click="pointManager(mode, 0, mode ? 5 : 6, true, 0)"><ion-icon :icon="addCircle"></ion-icon></ion-button>
              </div>
              

              <div class="flex items-center"> 
                <ion-button fill="clear" color="danger" size="small" @click="pointManager(mode, 1, mode ? 5 : 6, false, 0)"><ion-icon :icon="removeCircle"></ion-icon></ion-button>
                <div class="zstack w-12">
                  <img src="../images/cube.png">
                  <p>{{ mode ? teleCubesCones[0][1] : autoCubesCones[0][1] }}</p>
                </div>
                <ion-button fill="clear" color="success" size="small" @click="pointManager(mode, 1, mode ? 5 : 6, true, 0)"><ion-icon :icon="addCircle"></ion-icon></ion-button>
              </div>
            </div>

            <div class="flex [&>*]:mx-1 justify-evenly">
              <ion-text color="warning" class="text-center font-bold text-xl" style="writing-mode: vertical-rl; text-orientation: mixed;">Mid</ion-text>
              <div class="flex items-center"> 
                <ion-button fill="clear" color="danger" size="small" @click="pointManager(mode, 0, mode ? 3 : 4, false, 1)"><ion-icon :icon="removeCircle"></ion-icon></ion-button>
                <div class="zstack w-12">
                  <img src="../images/cone.png">
                  <p>{{ mode ? teleCubesCones[1][0] : autoCubesCones[1][0] }}</p>
                </div>

                <ion-button fill="clear" color="success" size="small" @click="pointManager(mode, 0, mode ? 3 : 4, true, 1)"><ion-icon :icon="addCircle"></ion-icon></ion-button>
              </div>
              

              <div class="flex items-center"> 
                <ion-button fill="clear" color="danger" size="small" @click="pointManager(mode, 1, mode ? 3 : 4, false, 1)"><ion-icon :icon="removeCircle"></ion-icon></ion-button>
                <div class="zstack w-12">
                  <img src="../images/cube.png">
                  <p>{{ mode ? teleCubesCones[1][1] : autoCubesCones[1][1] }}</p>
                </div>
                <ion-button fill="clear" color="success" size="small" @click="pointManager(mode, 1, mode ? 3 : 4, true, 1)"><ion-icon :icon="addCircle"></ion-icon></ion-button>
              </div>
            </div>

            <div class="flex [&>*]:mx-1 justify-evenly">
              <ion-text color="danger" class="text-center font-bold text-xl" style="writing-mode: vertical-rl; text-orientation: mixed;">Low</ion-text>
              <div class="flex items-center"> 
                <ion-button fill="clear" color="danger" size="small" @click="pointManager(mode, 0, mode ? 2 : 3, false, 2)"><ion-icon :icon="removeCircle"></ion-icon></ion-button>
                <div class="zstack w-12">
                  <img src="../images/cone.png">
                  <p>{{  mode ? teleCubesCones[2][0] : autoCubesCones[2][0] }}</p>
                </div>

                <ion-button fill="clear" color="success" size="small" @click="pointManager(mode, 0, mode ? 2 : 3, true, 2)"><ion-icon :icon="addCircle"></ion-icon></ion-button>
              </div>
              

              <div class="flex items-center"> 
                <ion-button fill="clear" color="danger" size="small" @click="pointManager(mode, 1, mode ? 2 : 3, false, 2)"><ion-icon :icon="removeCircle"></ion-icon></ion-button>
                <div class="zstack w-12">
                  <img src="../images/cube.png">
                  <p>{{ mode ? teleCubesCones[2][1] : autoCubesCones[2][1] }}</p>
                </div>
                <ion-button fill="clear" color="success" size="small" @click="pointManager(mode, 1, mode ? 2 : 3, true, 2)"><ion-icon :icon="addCircle"></ion-icon></ion-button>
              </div>
            </div>
            
            



            
            
          </div>
          
          
        </ion-content>

      </ion-modal>

      <ion-modal :isOpen="openModal3">

        <ion-header>
          <ion-toolbar>
            <ion-title>Start Position</ion-title>
            <ion-buttons slot="end">
              <ion-button @click="openModal3 = false" class="font-bold">Close</ion-button>
            </ion-buttons>

          </ion-toolbar>
        </ion-header>
            
        <ion-content class="ion-padding modal-content flex justify-center items-center bg-red-100">
            
            <div class="w-full h-5/6 flex items-center justify-center flex-col">
              <ion-label class="text-center font-bold">
                <h1 class="font-bold">Robot Starting Position:</h1>
                
                <h1 class="font-bold">{{startposition == 0 ? "Loading Zone" : startposition == 1 ? "Charge Station" : "Cable Cover" }}</h1>
              </ion-label>
              <br>
              <div class="zstack-no-center">
                <img src="../images/field.png"/>
                <div class=" h-full w-20 ml-24 flex flex-col justify-between [&>*]:whitespace-normal">
                  <ion-button class="font-bold" @click="startposition = 1" :color="startposition == 1 ? 'success':'primary'">Loading Zone</ion-button>
                  <ion-button class="font-bold" @click="startposition = 2" :color="startposition == 2 ? 'success':'primary'">Charge Station</ion-button>
                  <ion-button class="font-bold" @click="startposition = 3" :color="startposition == 3 ? 'success':'primary'">Cable Cover</ion-button>
                </div>
              </div>
              <br>
              <ion-button class="font-bold w-full" expand="block" @click="startposition = 0" :color="startposition == 0 ? 'danger':'primary'">Not on field</ion-button>
              
              
            </div>
            

            
        </ion-content>

      </ion-modal>




    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
  import {IonPopover, IonCheckbox, IonItemDivider, IonInput, IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButton, IonButtons, IonIcon, IonCardHeader, IonText, IonList, IonItemSliding, IonItem, IonLabel, IonItemOptions, IonItemOption, IonModal } from '@ionic/vue';
  import {add, addOutline, chevronDown, addCircle, removeCircle} from 'ionicons/icons'
  import {Haptics, ImpactStyle} from '@capacitor/haptics'
  import {ref, reactive, computed, onMounted} from 'vue'
  import {Storage, Drivers} from '@ionic/storage'
  import {v4 as uuidv4} from 'uuid'
  import { toNumber } from '@vue/shared';

  const openModal = ref(false)
  const openModal2 = ref(false)
  const openModal3 = ref(false)
  const startpiece = ref(0)


  const leftcomm = ref(false)



  const autoCubesCones = ref([[0, 0], [0, 0], [0, 0]])
  const teleCubesCones = ref([[0, 0], [0, 0], [0, 0]])
  const autoPts = ref(0)
  const telePts = ref(0)
  const autocs = ref(0)
  const telecs = ref(0)

  const pros = ref("")
  const cons = ref("")
  const other = ref("")

  const name = ref("")
  const tnumber = ref("")
  const mnumber = ref("")

  const mode = ref(false)
  const startposition = ref(0)
  
  const store = ref(new Storage({
    name: '__mydb',
    driverOrder: [Drivers.IndexedDB, Drivers.LocalStorage]
  }))

  onMounted(async () => {
    console.log("egg")
    await store.value.create()
    await store.value.set("milk", "cheese")
    store.value.get("milk").then((ref) => {
      console.log(ref)
    })
  })

  const storeNew = async () => {
    await store.value.set(
      uuidv4(),
      {
        name: name.value,
        tnum: parseInt(tnumber.value),
        mnum: parseInt(mnumber.value),
        lcom: leftcomm.value,
        stpc: startpiece.value,
        spos: startposition.value,
        agrd: autoCubesCones.value,
        tgrd: teleCubesCones.value,
        apts: autoPts.value,
        tpts: telePts.value,
        achg: autocs.value,
        tchg: telecs.value,
        pros: pros.value,
        cons: cons.value,
        othr: other.value,



      }
    )
  }

  const changeState = () => {
    openModal.value = !openModal.value
    console.log(openModal.value)
  }

  const pointManager = (teleop: boolean, type: number, addamount: number, mode: boolean, position: number) => {
    var max = 6

        if (type == 1) {
          max = 3
        }
        
        if (teleop) {


          if (mode) {
            if (position != 2 && teleCubesCones.value[position][type] < max) {
              teleCubesCones.value[position][type] += 1
              Haptics.impact({style: ImpactStyle.Light})

              if (teleop) {
                telePts.value += addamount
              }
              else {
                autoPts.value += addamount
              }

            }
            else if (position == 2 && (teleCubesCones.value[position][0]+teleCubesCones.value[position][1]) < 9) {
              teleCubesCones.value[position][type] += 1
              Haptics.impact({style: ImpactStyle.Light})

              if (teleop) {
                telePts.value += addamount
              }
              else {
                autoPts.value += addamount
              }
            }
            else {
              Haptics.notification()
            }
          }

          if (!mode) {
            if (teleCubesCones.value[position][type] > 0) {
              teleCubesCones.value[position][type] -= 1
              Haptics.impact({style: ImpactStyle.Light})

              if (teleop) {
                telePts.value -= addamount
              }
              else {
                autoPts.value -= addamount
              }
            }
            else {
              Haptics.notification()
            }
          }

   
          
          
        }

        else {
          if (mode) {
            if (position != 2 && autoCubesCones.value[position][type] < max) {
              autoCubesCones.value[position][type] += 1
              Haptics.impact({style: ImpactStyle.Light})

              if (teleop) {
                telePts.value += addamount
              }
              else {
                autoPts.value += addamount
              }

            }
            else if (position == 2 && (autoCubesCones.value[position][0]+autoCubesCones.value[position][1]) < 9) {
              autoCubesCones.value[position][type] += 1
              Haptics.impact({style: ImpactStyle.Light})

              if (teleop) {
                telePts.value += addamount
              }
              else {
                autoPts.value += addamount
              }
            }
            else {
              Haptics.notification()
            }
          }

          if (!mode) {
            if (autoCubesCones.value[position][type] > 0) {
              autoCubesCones.value[position][type] -= 1
              Haptics.impact({style: ImpactStyle.Light})

              if (teleop) {
                telePts.value -= addamount
              }
              else {
                autoPts.value -= addamount
              }
            }
            else {
              Haptics.notification()
            }
          }
        }
  }

</script>


