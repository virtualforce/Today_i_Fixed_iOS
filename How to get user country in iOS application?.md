# Problem
Some of the features in the application are available only in Canada, that's why need to identify the user's location.


# Environment
N/A

# How you fix it
=> There are a couple of ways to identify the user country i.e. using IP, user location, etc. I achieved this using user location via CLLocation


# Solution
Here are the main steps you need to follow to get the user location, First add the permission keys (provided below) in Info.plist, then import the **CoreLocation** in the relevant controller.
Then get the user's location using **CLLocationManager** instance. After that use **reverseGeocodeLocation** method of **CLGeocoder** to get the Placemark. 
Below is the code snippet for further clarification and help.

# Info.plist
```
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>App_Name requires user’s location for better user experience.</string>

<key>NSLocationWhenInUseUsageDescription</key>
<string>App_Name requires user’s location for better user experience.</string>
```

# LoctionViewController (Where need Location)
```
// MARK: - Get user location
extension LoctionViewController {

    func getUserCountry() {
        
        let locationManager = CLLocationManager()
        locationManager.requestWhenInUseAuthorization()
        
        if CLLocationManager.authorizationStatus() == .authorizedWhenInUse {
            locationManager.startUpdatingLocation()
        }
        
        guard let location = locationManager.location else {
            print("Failed to obtain user location.")
            return
        }
        
        let geocoder = CLGeocoder()
        geocoder.reverseGeocodeLocation(location) { (placemarks, error) in
            guard let placemark = placemarks?.first,
                  let country = placemark.country else {
                print("Failed to geocode user location.")
                return
            }
            
            print(country) // eg. United States
            print(placemark.locality ?? "locality not found") // city, eg. Cupertino
            print(placemark.subLocality ?? "subLocality not found") // neighborhood, common name, eg. Mission District
            print(placemark.administrativeArea ?? "administrativeArea not found") // state, eg. CA
            print(placemark.subAdministrativeArea ?? "subAdministrativeArea not found") // county, eg. Santa Clara
            print(placemark.postalCode ?? "postalCode not found") // zip code, eg. 95014
            print(placemark.isoCountryCode ?? "isoCountryCode not found") // eg. US
            
        }
    }
}

```
