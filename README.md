# React Native Map with real time location selection for Android

![Screenshot](https://github.com/codemaker2015/React-native-map-view/blob/master/demo/demo.gif)

These days almost every mobile app needs the map integration for a purpose. React Native gives a great ability to serve this purpose easily. Today we will see how to integrate google map in and how to use google map platform https://maps.googleapis.com to get the real time location by moving around the map view.

### Create new project using react-native-cli
First thing you need to have react-native-cli installed globally on your development machine. You can install it by using npm command :

    npm install -g react-native-cli

After installing react-native-cli create a new project using command:

    react-native init ReactNativeMapView

This will generate a boilerplate for you with a some pre-configured dependencies. Here are the dependencies at the time of building this project:

    "react": "16.8.3",
    "react-native": "0.59.5"

You can run your app using `react-native run-android`.

### Install and configure react-native-maps
Now let’s install react-native-maps using npm command: 

    npm install react-native-maps --save. 
    
The library ships with platform native code that needs to be compiled together with React Native. This requires you to configure your build tools. The detailed installation instructions are given here, we will do the setup by using react native link. Run 

    react-native link react-native-maps

this will link the package with our native app(for both Android and iOS). 

### Add permissions for map view in our app
Before writing code to display map in our app, we need to get make sure that the required permissions are configured and permitted by the user. To do that add below set of permissions in `android/app/src/main/AndroidManifest.xml file`.

    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

Also, we need to add add google’s Geo-location API key. Generate your own API key from [Google Maps Platform](https://cloud.google.com/maps-platform/)
Add below line of code in the same file inside `<application>` tag at the end.

    <meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="<<YOUR API KEY GOES HERE>>"/>
    
### Write a code to display map view
Now it’s time to write some code to display the real time map view in our app.

    constructor(props) {
      super(props);
      this.state = {
        loading: true,
        region: {
        latitude: 10,
        longitude: 10,
        latitudeDelta: 0.001,
        longitudeDelta: 0.001
      }
      };
    }
    
    render() {
      return (
    <MapView
        style={styles.map}
        initialRegion={this.state.region}
        showsUserLocation={true}
        onMapReady={this.onMapReady}
        onRegionChangeComplete={this.onRegionChange}>
      <MapView.Marker
        coordinate={{ "latitude": this.state.region.latitude,   
        "longitude": this.state.region.longitude }}
        title={"Your Location"}
        draggable />
    </MapView>);
    }

### Add custom marker using font
Many times we see a center positioned icon on the map, which is fixed at the center and we see the map moved around it. To achieve this we will need to add a custom marker using icon or an image. Here we will show icon as a marker and to do so we need to configure and add font. Follow below steps to add fontawesome fonts,
Setp 1: Download fontawesome-webfont.ttf file and rename the file to fontawesome.ttf.
Step 2: create `/assets/fonts/` directory in your project directory at root level.
Step 3: paste fontawesome.ttf file to `/assets/fonts/`
Step 4: add below code to your package.json file.

    "rnpm": {
      "assets": [
      "./assets/fonts"
      ]
    }
    
Step 5: run `react-native link`
Now that we have fonts configured, we can add a font as a marker.

    <View>
      <MapView
      initialRegion={this.state.region}
      showsUserLocation={true}
      >
      </MapView>
      <View style={styles.mapMarkerContainer}>
        <Text style={{ fontFamily: 'fontawesome', fontSize: 42, color:  
          "#ad1f1f" }}>&#xf041;</Text>
        </View>
      </View>
    </view>

### Get location details from the map
We can get the location details using [google’s map API](https://maps.googleapis.com/). All we need to provide is latitude, longitude and API key to the https://maps.googleapis.com.
Current latitude and longitude can be captured by using `onRegionChangeComplete` prop method provided by MapView.
