# Adaptive Layouts in iOS
Notes from experimenting with and reading about Adaptive Layouts which were introduced in iOS8

## Key points
* Adaptive Layouts are abstract layouts based on relationships between various UI-elements
* Adaptive Layouts allows apps to adapt their layout to a particular device and orientation
* Uses __Auto Layout__, but with the addition of `universal storyboards`, `size classes`, and `trait collections`
* Designing for an abstract size class allows your app to transfer seemlessly to current (and future) iOS devices

## Universal Storyboards
* Instead of keeping different storyboards for different devices, you can now design entirely _different layouts_ all within _one storyboard_ file
* __View Controllers__ are now square, which forces you to _design with relative positions_ in mind - instead of a _specific device_
* The view controller will resize to match the current device it is being run on

### Preview Assistant Editor
* The __Preview Assistant Editor__ displays a _preview of your layout_ on a number of _different device sizes_, in _both orientations_, _updating in real time_ as you update your storyboard
* Turn it on by opening the `Assistant Editor`, selecting `Preview`, and then your storyboard file
* You can add more device sizes by tapping the `+` in the lower left corner of the preview window
* You can change the orientation by tapping the button next to the device name underneath a specific size preview

## Trait Collections
* A __trait collection__ is a set of characteristics that describe the current environment
* Trait collections includes the vertical and horizontal size class,  the screen scale, and the device idiom


### Size Classes
* A __size class__ is a property applied to any view or controller, that represents the amount of content that can be displayed in a given horizontal or vertical dimension
* Size classes are automatically handed down to the app by the device, but _can be overridden_ for individual views/ controllers
* There are two size classes for each dimension: __regular__ and __compact__
* The following table shows how the different size classes match the different devices and orientations:

|Device/Orientation |Vertical Size Class|Horizontal Size Class|
|-------------------|-------------------|---------------------|
|iPad Portrait      |Regular            |Regular              |
|iPad Landscape     |Regular            |Regular              |
|iPhone Portrait    |Regular            |Compact              |
|iPhone Landscape   |Compact            |Compact              |
|iPhone 6+ Landscape|Compact            |Regular              |

* You can change the current size class of your storyboard by tapping the bottom bar (`wAny hAny`), and selecting cells in the grid
* You create different layouts by __uninstalling__ (deactivating) and __installing__ (activating) constraints for specific size classes, they will only take effect on devices with that size class
	* The size class `wAny hAny` applies to any size classes that haven't been specified
	* You can uninstall a constraint by selecting it in the __size inspector__ and pressing `delete`, or from the storyboard/ document outline and pressing `cmd + delete`
		* The constraint will appear _greyed out_ in both the size inspector and document outline
	* By opening the size inspector for a given constraint you can also install/ uninstall it for different size classes
* __Fonts__ can also be overridden with size classes, by pressing the little `+` next to the font name in the __Attributes Inspector__
* Entire views can also be uninstalled in certain size classes, by selecting them and pressing `cmd + delete`
* You can override a size class for a view or controller using the method: `setOverrideTraitCollection(UITraitCollection(...), forChildViewController: ViewController)`
	* Providing `nil` as the first parameter, will reset the trait collection back to the default for this device
* __Asset Catalogs__ allow you to specify different images for different size classes
	* This can be set in the Attributes Inspector
	* The notation for the different size classes is as follows: `*` = Any, `-` = Compact, `+` = Regular
* `UIAppearance` allows you to specify different styles based on the size classes: `object.appearanceForTraitCollection(traitCollection).attribute = value`