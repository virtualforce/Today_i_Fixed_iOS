## Problem
How to call delegates of UIKIT controls in SwiftUI?

## How you fix it
We can call UIKit controls delegate methods in SwiftUI by using coordinators which are designed to act as delegates for UIKit view controllers. As we know, the “delegates” are objects that respond to events that occur elsewhere. For example, UIKit lets us attach a delegate object to its map view, and that delegate will be notified when the user moves map view. This meant that UIKit developers could navigate and utilize maps functionality without creating any new custom class.
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
          Step 3: Assign delegate to content.coordinator
        mapView.delegate = context.coordinator
        return mapView
    }
    
    func updateUIView(_ uiView: GMSMapView, context: Context) {
        uiView.animate(toLocation: CLLocationCoordinate2D(latitude: location?.coordinate.latitude ?? 0.0, longitude:             location?.coordinate.longitude ?? 0.0))
    }
    
    //Step 1: Create Coordinator class within UIViewRepresentable type struct and inhereited from NSOBject and GMSMapViewDelegate
    class Coordinator: NSObject, GMSMapViewDelegate {
        var parent: GoogleMapsView
        
        init(_ parent: GoogleMapsView) {
            self.parent = parent
        }
  
  Step 2: Create makeCoordinator and return Coordinator() class instance. SwiftUI autonatically call makeCoordinator method.
func makeCoordinator() -> Coordinator {
           Coordinator(self)
    }

Step 4: Implement delegate method and utilise when some event occur on Map view
func mapView(_ mapView: GMSMapView, idleAt position: GMSCameraPosition) {
            
            mapView.animate(toZoom: 16)
            let latitude = mapView.camera.target.latitude
            let longitude = mapView.camera.target.longitude
            let coordinate = CLLocationCoordinate2D(latitude: latitude, longitude: longitude)
            self.parent.placeMarkerOnCenter(centerMapCoordinate: coordinate, mapView: mapView)
            self.parent.location = CLLocation(latitude: latitude, longitude: longitude)
        }
}
```
