Se creo un  nuevo proyecto iónico / angular para el trabajo de taller dispositivos móviles 
En este proyecto, se instalo y puso a prueba la principal funcionaliad de la aplicacion, el cual es la geolocalización y posición de guardias.

Utilice el comando para instalar Cordova globalmente en la maquina.

npm install -g cordova ionic

Instalé una nueva aplicación Ionic Angular con la ayuda de Ionic CLI, ejecuté el siguiente comando en el terminal.

ionic start tareaTaller-app blank --type=angular
Carpeta del proyecto.

cd tareaTaller
Inicie la aplicación en el navegador.

ionic serve --lab
Instalación y configuración Cordova Geolocation, Geocoder y complementos nativos iónicos
En este paso, primero se instaló y luego configuró los complementos nativos Cordova Geolocation, Geocoder e Ionic en una aplicación Ionic 5.

Geolocalización
los cordova-plugin-geolocalización La API del complemento se basa en la Especificación de la API de geolocalización de W3C y ayuda a obtener los datos de ubicación (latitud y longitud).

Este complemento proporciona información sobre la ubicación del dispositivo, como latitud y longitud. Las fuentes comunes de información de ubicación incluyen el Sistema de Posicionamiento Global (GPS) y la ubicación inferida de señales de red como la dirección IP, RFID, direcciones MAC de WiFi y Bluetooth, e ID de celda GSM / CDMA.

ionic cordova plugin add cordova-plugin-geolocation
Instale @ ionic-native / geolocation a través de npm usando el siguiente comando.

npm install @ionic-native/geolocation

Este complemento de ubicación geográfica proporciona datos del dispositivo, como la ubicación, la latitud y la longitud del dispositivo. Las fuentes comunes de información de ubicación incluyen el Sistema de Posicionamiento Global (GPS) y la ubicación inferida de señales de red como la dirección IP, RFID, direcciones MAC de WiFi y Bluetooth, e ID de celda GSM / CDMA.

Geocodificador nativo
El complemento de geocodificador nativo ayuda a obtener la dirección para las coordenadas dadas y también realiza la codificación geográfica hacia adelante y hacia atrás para las plataformas iOS y Android.

ionic cordova plugin add cordova-plugin-nativegeocoder

npm install @ionic-native/native-geocoder
Importar complementos en el módulo de aplicación principal
Agregué los complementos anteriores en el archivo principal del módulo de la aplicación Ionic, abra app.module.ts e importar y agregar complementos en un importar formación.



import  NgModule  from '@angular/core';
import  BrowserModule  from '@angular/platform-browser';
import  RouteReuseStrategy  from '@angular/router';

import  IonicModule, IonicRouteStrategy  from '@ionic/angular';
import  SplashScreen  from '@ionic-native/splash-screen/ngx';
import  StatusBar  from '@ionic-native/status-bar/ngx';

import  AppComponent  from './app.component';
import  AppRoutingModule  from './app-routing.module';


import  Geolocation  from '@ionic-native/geolocation/ngx';
import  NativeGeocoder  from '@ionic-native/native-geocoder/ngx';

@NgModule(
  declarations: [AppComponent],
  entryComponents: [],
  imports: [BrowserModule, IonicModule.forRoot(), AppRoutingModule],
  providers: [
    StatusBar,
    SplashScreen,
     provide: RouteReuseStrategy, useClass: IonicRouteStrategy ,
    Geolocation,
    NativeGeocoder
  ],
  bootstrap: [AppComponent]
)

export class AppModule 

En este apartado, no pude obtener  la latitud y longitud de la ubicación del dispositivo del usuario actual mediante la geolocalización. Describiré lo que agregue para obtener la posición del dispositivo del usuario al abrir home.pge.ts archivo y agregar el código, que sin embargo no funcionó.


import  Component, NgZone  from '@angular/core';
import  Geolocation  from '@ionic-native/geolocation/ngx';

@Component(
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
)

export class HomePage 
  latitude: any = 0; 
  longitude: any = 0; 

  constructor(
    private geolocation: Geolocation
  ) 

  options = 
    timeout: 10000, 
    enableHighAccuracy: true, 
    maximumAge: 3600
  ;

  
  getCurrentCoordinates() 
    this.geolocation.getCurrentPosition().then((resp) => 
      this.latitude = resp.coords.latitude;
      this.longitude = resp.coords.longitude;
     ).catch((error) => 
       console.log('Error getting location', error);
     );
  
Agregue el siguiente código en home.page.html expediente.

// home.page.html

<ion-header>
  <ion-toolbar>
    <ion-title>
        Taller Dispositivos Moviles
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
  <div class="ion-padding">

    <ion-button (click)="getCurrentCoordinates()" expand="block">
     Obtener Ubicación
    </ion-button>

  <ion-list>
    <ion-list-header>
      <ion-label>Ubicación en Coordenadas</ion-label>
    </ion-list-header>
    <ion-item>
      <ion-label>
       
        Latitud
      </ion-label>
      <ion-badge color="danger" slot="end">Latitud</ion-badge>
    </ion-item>
    <ion-item>
      <ion-label>
        Longitud
      </ion-label>
      <ion-badge color="danger" slot="end">Longitud</ion-badge>
    </ion-item>
    </ion-list>    
  </div>
</ion-content>
