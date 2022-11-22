## Problem
How to use UIKIT controls in SwiftUI??

## How you fix it
Despite SwiftUI providing many of UIKit’s UIView subclasses, but it doesn’t have them all yet at this time. Fortunately, it’s not hard to create custom wrappers for a UIView that you want.
let’s create a simple SwiftUI wrapper for GoogleMapsView to display google maps within SwiftUI View.
This takes four steps:
    1    Creating a struct that conforms to UIViewRepresentable.
    2    Defining one property that stores the location we are working with.
    3    Giving it a makeUIView() method that will return our map view.
    4    Adding a updateUIView() method that will be called whenever the data for the map view has changed.
 

## Solution
```
import GoogleMaps

struct GoogleMapsView: UIViewRepresentable {

var cameraLocation: CLLocationCoordinate2D?
    var centerMapCoordinate:CLLocationCoordinate2D!
    var mapView: GMSMapView?
    @Binding var location: CLLocation?
    
    func makeUIView(context: Context) -> GMSMapView {
        
        let mapView = GMSMapView(frame: CGRect.zero, camera: location)
        mapView.delegate = context.coordinator
        return mapView
    }
    
    func updateUIView(_ uiView: GMSMapView, context: Context) {
        uiView.animate(toLocation: CLLocationCoordinate2D(latitude: location?.coordinate.latitude ?? 0.0, longitude:             location?.coordinate.longitude ?? 0.0))
    }
}
```
