<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1 Page Header</ion-title>
					<ion-buttons slot="end">
						<ion-button class="" color="primary" @click="scan()">Scan</ion-button>
					</ion-buttons>

      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">

			<div v-if="scanResult">
				<p class="ion-padding">{{scanResult}}</p>
				<ion-button class="ion-margin-start" color="primary" @click="read()">Browser</ion-button>
			</div>

    </ion-content>
  </ion-page>
</template>

<script lang="ts">
		import { defineComponent } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton, IonButtons } from '@ionic/vue';
import { BarcodeScanner } from '@capacitor-community/barcode-scanner';
import { Browser } from '@capacitor/browser';

export default  defineComponent({
		name: 'Tab1Page',
		ionViewWillEnter() {
				BarcodeScanner.prepare();
		},
		components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonButton, IonButtons },
		data() {
				return {
						scanResult: 'http://www.sogou.com',
						async read() {
								await Browser.open({ url: this.scanResult });
						},
						async scan() {
								BarcodeScanner.hideBackground(); // make background of WebView transparent
								console.log('CLick scan');

								document.body.classList.add("qrscanner"); // add the qrscanner class to body

								const result = await BarcodeScanner.startScan(); // start scanning and wait for a result

								// if the result has content
								if (result.hasContent) {
										console.log(result.content); // log the raw scanned content
										this.scanResult = result.content;
										console.log('Click scan')
								}
								document.body.classList.remove("qrscanner"); // remove the qrscanner from the body
						}
				}
		},
		computed: {
		}
});
</script>
