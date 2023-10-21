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
        <ion-item-sliding color="medium" v-for="(val) in datacontent">
          <ion-item>
            <ion-label>
              <h1 style="font-weight: bold">Match {{ val[1].mnum }}, Team {{ val[1].tnum }}</h1>
              <p>Scouter Name: {{ val[1].name }}</p>              
            </ion-label>
          </ion-item>
          <ion-item-options ref="cardoptions">
            <ion-item-option @click="formLinkGen(val[1])">
              <ion-icon slot="top" :icon="globeOutline"></ion-icon>
              Resend
            </ion-item-option>
            <ion-item-option color="danger" @click="removeItem(val)">
              <ion-icon slot="top" :icon="trashOutline"></ion-icon>
              Delete
            </ion-item-option>
          </ion-item-options>
        </ion-item-sliding>

        

       
      </ion-list>




      <ion-modal :is-open="openModal">
        <ion-header>
          <ion-toolbar>
            <ion-title>Match Data</ion-title>
            <ion-buttons slot="end">
              <ion-button @click="storeNew()" class="font-bold" :disabled="!(name && tnumber && mnumber)">Add</ion-button>
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

            <ion-item class="clickable" @click="openModal3 = true">
              <p>Start Position</p>
            </ion-item>

            <ion-item>
              <ion-checkbox color="success" v-model="leftcomm">Did leave community?</ion-checkbox>
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
            

            <!-- <ion-item class="clickable" @click="{openModal2 = true; mode = true}">
              <p>Cubes and Cones</p>
            </ion-item> -->

            <ion-item>
              <ion-checkbox color="danger" v-model="fell">Did they fall?</ion-checkbox>
            </ion-item>

            <ion-item>
              <div class="w-full flex items-center justify-between">
                <p>How well can they hold cones?</p>
                <select v-model="conehold" class="text-neutral-500 outline-none w-28 whitespace-pre-wrap my-3">
                  <option value="0">Can't grab</option>
                  <option value="1">Drops</option>
                  <option value="2">Drops on bump</option>
                  <option value="3">Does not drop</option>  
         
                </select>
                
              </div>
            </ion-item>

            <ion-item>
              <div class="w-full flex items-center justify-between">
                <p>How well can they hold cubes?</p>
                <select v-model="cubehold" class="text-neutral-500 outline-none w-28 whitespace-pre-wrap my-3">
                  <option value="0">Can't grab</option>
                  <option value="1">Drops</option>
                  <option value="2">Drops on bump</option>
                  <option value="3">Does not drop</option>  
         
                </select>
                
              </div>
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
              <div class="w-full flex items-center justify-between">
                <p>Driver Performance</p>
                <select v-model="drivrating" class="text-neutral-500 outline-none w-28 whitespace-pre-wrap my-3">
                  <option value="0">Slow</option>
                  <option value="1">Fluid</option>
                  <option value="2">Aggressive</option>
              
         
                </select>
                
              </div>
            </ion-item>
            <ion-item>
              <div class="w-full flex items-center justify-between">
                <p>Tippiness Rating</p>
                <select v-model="tippiness" class="text-neutral-500 outline-none w-28 whitespace-pre-wrap my-3">
                  <option value="0">Very Tippy</option>
                  <option value="1">Tippy</option>
                  <option value="2">Decent</option>
                  <option value="3">Stable</option>
                  <option value="4">Very Stable</option>
         
                </select>
                
              </div>
            </ion-item>

            <ion-item>
              <div class="w-full flex items-center justify-between">
                <p>Preferred Substation</p>
                <select v-model="substation" class="text-neutral-500 outline-none w-28 whitespace-pre-wrap my-3">
                  <option value="0">Single Substation</option>
                  <option value="1">Double Substation</option>         
                </select>
                
              </div>
            </ion-item>

            <ion-item>
              <ion-textarea @input="pros=$event.target.value" class="my-1" label="" placeholder="What did you like about their performance?"></ion-textarea>
            </ion-item>

            <ion-item>
              <ion-textarea @input="cons=$event.target.value" class="my-1" label="" placeholder="What did you dislike about their performance?"></ion-textarea>
            </ion-item>

            <ion-item>
              <ion-textarea @input="other=$event.target.value" class="my-1" label="" placeholder="Other comments"></ion-textarea>
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
              <p>{{ currenttime }}</p>
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
            <div>
              <ion-list :inset="true" v-if="mode">
                <ion-item>
                  <div class="w-full flex items-center justify-between px-1">
                    <p>Number of times the failed to defend</p>
                  
                    <ion-button fill="clear" @click="whiffs ? whiffs -= 1 : whiffs ">
                      <ion-icon color="danger" :icon="removeOutline" size="small"/>
                    </ion-button>
                    <p>{{ whiffs }}</p>
                    <ion-button fill="clear" @click="whiffs += 1">
                      <ion-icon color="success" :icon="addOutline" size="small"/>
                    </ion-button>
                  </div>
                </ion-item>

                <ion-item>
                  <div class="w-full flex items-center justify-between px-1">
                    <p>Number of times they defended</p>
                  
                    <ion-button fill="clear" @click="defended ? defended -= 1 : defended ">
                      <ion-icon color="danger" :icon="removeOutline" size="small"/>
                    </ion-button>
                    <p>{{ defended }}</p>
                    <ion-button fill="clear" @click="defended += 1">
                      <ion-icon color="success" :icon="addOutline" size="small"/>
                    </ion-button>
                  </div>
                </ion-item>

                <ion-item>
                  <div class="w-full flex items-center justify-between px-1">
                    <p>Number of times they inhibited their team</p>
                  
                    <ion-button fill="clear" @click="inhibition ? inhibition -= 1 : inhibition ">
                      <ion-icon color="danger" :icon="removeOutline" size="small"/>
                    </ion-button>
                    <p>{{inhibition}}</p>
                    <ion-button fill="clear" @click="inhibition += 1" class="bg-transparent">
                      <ion-icon color="success" :icon="addOutline" size="small"/>
                    </ion-button>
                  </div>
                </ion-item>
              </ion-list>
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

<!--end of templating code, this is now js from here on out-->

<script setup lang="ts">
  import {IonPopover, IonCheckbox, IonTextarea, IonItemDivider, IonInput, IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButton, IonButtons, IonIcon, IonCardHeader, IonText, IonList, IonItemSliding, IonItem, IonLabel, IonItemOptions, IonItemOption, IonModal } from '@ionic/vue';
  import {add, addOutline, chevronDown, addCircle, removeCircle, removeOutline, flashlight, trashOutline, globeOutline} from 'ionicons/icons'
  import {Haptics, ImpactStyle} from '@capacitor/haptics'

  import {ref, reactive, computed, onMounted} from 'vue'
  import type {Ref} from 'vue'

  import {Storage, Drivers} from '@ionic/storage'
  import {v4 as uuidv4} from 'uuid'
  import { toNumber } from '@vue/shared';

  import axios from 'axios'

  const openModal = ref(false)
  const openModal2 = ref(false)
  const openModal3 = ref(false)
  const startpiece = ref(0)

  const whiffs = ref(0)
  const inhibition = ref(0)
  const defended = ref(0)

  const leftcomm = ref(false)

  const autoCubesCones = ref([[0, 0], [0, 0], [0, 0]])
  const teleCubesCones = ref([[0, 0], [0, 0], [0, 0]])
  const autoPts = ref(0)
  const telePts = ref(0)
  const autocs = ref(0)
  const telecs = ref(0)

  const drivrating = ref(0)
  const tippiness = ref(0)
  const substation = ref(0)

  const cubehold = ref(0)
  const conehold = ref(0)

  const pros = ref("")
  const cons = ref("")
  const other = ref("")


  const currenttime = ref(0)
  const cycletimearray: Ref<Array<number>> = ref([])

  const name = ref("")
  const tnumber = ref("")
  const mnumber = ref("")

  const mode = ref(false)
  const startposition = ref(0)


  const fell = ref(false)
  
  const store = ref(new Storage())

  const datacontent: Ref<Array<any>> = ref([])

  const cardoptions = ref()

 

  store.value.create()
 

  onMounted(async () => {
    store.value.forEach((val, k, index) => {
      datacontent.value.push([k, JSON.parse(val)])
      console.log(datacontent.value)
    })


    setInterval(
      () => {
        if (openModal2.value) {
          currenttime.value++
        }
        
      },
      1000
    )
  })



  const averageCycle = computed(() => {

    var total = 0


    cycletimearray.value.forEach((val) => {total += val})

    var average = (total/cycletimearray.value.length)

    return average ? Math.round(average) : 0
  })

  const storeNew = async () => {


    //datastructure that signifies the objects

    var datastruct = {
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


        cycl: averageCycle.value,

        fail: whiffs.value,
        inhb: inhibition.value,
        dfnd: defended.value,

        rate: drivrating.value,
        tips: tippiness.value,
        sbst: substation.value,

        fell: fell.value,
        cogb: conehold.value,
        cugb: cubehold.value,

        pros: pros.value,
        cons: cons.value,
        othr: other.value
    }


    var uuid = uuidv4()

    // datacontent.value.push([uuid, datastruct])
    datacontent.value.push([uuid, datastruct])
    await store.value.set(uuid, JSON.stringify(datastruct))

    formLinkGen(datastruct)

    //reset all values back to standard
    name.value = ""
    tnumber.value = ""
    mnumber.value = ""
    leftcomm.value = false
    startpiece.value = 0
    startposition.value = 0
    autoCubesCones.value = [[0, 0], [0, 0], [0, 0]]
    teleCubesCones.value = [[0, 0], [0, 0], [0, 0]]
    autoPts.value = 0
    telePts.value = 0

    

    autocs.value = 0
    telecs.value = 0
    whiffs.value = 0
    inhibition.value = 0
    defended.value = 0
    pros.value = ""
    cons.value = ""
    other.value = ""

    fell.value = false
    conehold.value = 0
    cubehold.value = 0
    drivrating.value = 0
    substation.value = 0
    tippiness.value = 0
    cycletimearray.value = []
    currenttime.value = 0

    console.log(pros.value)
    console.log(cons.value)
    console.log(other.value)

    openModal.value = false
  }

  const removeItem = async(item: Array<any>) => {
    await store.value.remove(item[0])
    datacontent.value.splice(datacontent.value.indexOf(item), 1)
    cardoptions.value.$el.close()
  }


  const formLinkGen = (data: Object) => {
    
    var nm = "entry.2126424725"
    var team = "entry.1373155014"
    var match = "entry.733521020"

    var spos = "entry.901381857"
    var stpc = "entry.1749280002"

    var oocom = "entry.897955818"


    var at = "entry.443174066" //not working
    var am = "entry.703504752" //not working
    var al = "entry.835867991" //not working

    var a = [
      "entry.618327558", 
      "entry.1104917121", 
      "entry.1558545009",
      "entry.491145462",
      "entry.897281594",
      "entry.808988189"
    ] //not working

    var autochg = "entry.1638054401"


    var t = ["entry.1021364095", "entry.2035365430", "entry.1556845106"]

    var telechg = "entry.1864436619"


    var tip = "entry.287809036"
    var fall = "entry.102646935"

    var conegrab = "entry.1393322075"
    var cubegrab = "entry.78309013"



    var whiff = "entry.1688133843"
    var defend = "entry.635793247"
    var inhibit = "entry.1748152210"

    var drivrating = "entry.89792760" //not working
    var cycletime = "entry.1124506895"
    var subst = "entry.1270495342"

    var pro = "entry.534050999"
    var con = "entry.820214933"
    var others = "entry.844437757"     
    
    
    var formID = "1FAIpQLScSFIdAgkF4KTI1nrBYgVWXfQSRSZrQdSsIWagp1jh4XXsphw"

    
    //formResponse
    var baseLinkarr = ["https://docs.google.com/forms/d/e/1FAIpQLScSFIdAgkF4KTI1nrBYgVWXfQSRSZrQdSsIWagp1jh4XXsphw/formResponse?usp=pp_url/"]



    baseLinkarr.push(`&${nm}=${plusEncoding(data.name)}`)
    baseLinkarr.push(`&${team}=${data.tnum}`)
    baseLinkarr.push(`&${match}=${data.mnum}`)



    if (data.stpc == 0) {
      baseLinkarr.push(`&${stpc}=None`)
    }
    else if (data.stpc == 1) {
      baseLinkarr.push(`&${stpc}=Cone`)
    }
    else {
      baseLinkarr.push(`&${stpc}=Cube`)
    }


    if (data.spos == 1) {
      baseLinkarr.push(`&${spos}=Loading+Zone`)
    }
    else if (data.spos == 2) {
      baseLinkarr.push(`&${spos}=Charging+Station`)
    }
    else {
      baseLinkarr.push(`&${spos}=Cable+Cover`)
    }

    if (data.achg == 0) {
      baseLinkarr.push(`&${autochg}=Neither+0+pts`)
    }
    else if (data.achg == 1) {
      baseLinkarr.push(`&${autochg}=Docked+8+pts`)
    }
    else {
      baseLinkarr.push(`&${autochg}=Balanced+12+pts`)
    }

    if (data.tchg == 0) {
      baseLinkarr.push(`&${telechg}=Neither+0+pts`)
    }
    else if (data.tchg == 1) {
      baseLinkarr.push(`&${telechg}=Docked+6+pts`)
    }
    else {
      baseLinkarr.push(`&${telechg}=Balanced+10+pts`)
    }

  

    if (data.lcom) {
      baseLinkarr.push(`&${oocom}=Yes`)
    }
    else {
      baseLinkarr.push(`&${oocom}=No`)
    }

    baseLinkarr.push(`&${whiff}=${data.fail}`)
    baseLinkarr.push(`&${inhibit}=${data.inhb}`)
    baseLinkarr.push(`&${defend}=${data.dfnd}`)


    console.log(`driver rating: ${data.rate}`)

    if (data.rate === 0) {
      baseLinkarr.push(`&${drivrating}=Slow`)
      console.log("trigger")
    }
    else if (data.rate === 1) {
      baseLinkarr.push(`&${drivrating}=Fluid`)
      console.log("trigger")
    }
    else {
      baseLinkarr.push(`&${drivrating}=Fast+but+Bumps+into+Things`)
      console.log("trigger")
    }

    console.log

    baseLinkarr.push(`&${tip}=${parseInt(data.tips)+1}`)

    if (data.fell) {
      baseLinkarr.push(`&${fall}=Yes`)
    }
    else {
      baseLinkarr.push(`&${fall}=No`)
    }

    baseLinkarr.push(`&${cycletime}=${data.cycl}`) 

    if(data.pros != "") {
      baseLinkarr.push(`&${pro}=${data.pros}`)
    }

    if(data.cons != "") {
      baseLinkarr.push(`&${con}=${data.cons}`) 
    }

    if(data.othr != "") {
      baseLinkarr.push(`&${others}=${data.othr}`)
    } 

    

    if (data.cogb == 0) {
      baseLinkarr.push(`&${conegrab}=Can't+Pick+it+up`) 
    }
    else if (data.cogb == 1) {
      baseLinkarr.push(`&${conegrab}=Drops+Immediately`)
    }
    else if (data.cogb == 2) {
      baseLinkarr.push(`&${conegrab}=Takes+a+bump+to+knock+it+out`) 
    }
    else {
      baseLinkarr.push(`&${conegrab}=Never+Drop`)
    }

    if (data.cugb == 0) {
      baseLinkarr.push(`&${cubegrab}=Can't+Pick+it+up`) 
    }
    else if (data.cugb == 1) {
      baseLinkarr.push(`&${cubegrab}=Drops+Immediately`)
    }
    else if (data.cugb == 2) {
      baseLinkarr.push(`&${cubegrab}=Takes+a+bump+to+knock+it+out`) 
    }
    else {
      baseLinkarr.push(`&${cubegrab}=Never+Drop`)
    }

    if (data.sbst == 0) {
      baseLinkarr.push(`&${subst}=Single+Substation`)
    }
    else {
      baseLinkarr.push(`&${subst}=Double+Substation`)
    }



    //check from here

    //named because its like the rosetta stone of numbers to positions huhuhu
    var rosetta = ["L1", "L2", "L3", "M1", "M2", "M3", "R1", "R2", "R3"]

    var autoposition = 0
    var teleposition = 0

  

 

    baseLinkarr.push(`&${a[0]}=${data.agrd[0][0]}`)
    baseLinkarr.push(`&${a[1]}=${data.agrd[1][0]}`)
    baseLinkarr.push(`&${a[2]}=${data.agrd[2][0]}`)
    baseLinkarr.push(`&${a[3]}=${data.agrd[0][1]}`)
    baseLinkarr.push(`&${a[4]}=${data.agrd[1][1]}`)
    baseLinkarr.push(`&${a[5]}=${data.agrd[2][1]}`)
 





    // var autogridcount = data.agrd.map((item: Array<any>) => {
    //   return item[0]+item[1]
    // })

    // console.log(autogridcount)
    
    // autogridcount.forEach((i: number) => {
    //   [...Array(i).keys()].forEach((j: number) => {
    //     baseLinkarr.push(`&${a[autoposition]}=${rosetta[j]}`) 
    //     console.log(j)
    //     console.log(rosetta[j])
    //   })

    //   autoposition++

    //   //to here for errors in the auto posit system
    // }) 

    // var telegridcount = data.tgrd.map((item: Array<any>) => {
    //   return item[0]+item[1]
    // })

    // console.log(telegridcount)
    
    // telegridcount.forEach((i: number) => {
    //   [...Array(i).keys()].forEach((j: number) => {
    //     baseLinkarr.push(`&${t[teleposition]}=${rosetta[j]}`) 
    //     console.log(j)
    //     console.log(rosetta[j])
    //   })
    //   teleposition++
    // }) 




    var baseLink = baseLinkarr.join("")
    console.log(baseLinkarr)

    console.log(baseLink)


    axios.get(baseLink).then(resp => {
      console.log(resp)
    })
    return baseLink
  }

  const plusEncoding = (input: string) => {
    var output = input.split(" ").join("+")
    console.log(output)
    return output
    
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
              cycletimearray.value.push(currenttime.value)
              currenttime.value = 0

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
              cycletimearray.value.push(currenttime.value)
              currenttime.value = 0
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
              cycletimearray.value.push(currenttime.value)
              currenttime.value = 0
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
              cycletimearray.value.push(currenttime.value)
              currenttime.value = 0
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


