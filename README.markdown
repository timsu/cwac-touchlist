CWAC TouchListView: Dragging and Dropping for Fun and Profit!
=========================================================
The standard `ListView` is a very sophisticated tool, but it
lacks any sort of drag-and-drop support. The Android open
source project has an example of implementing drag-and-drop
on a `ListView`, in the form of the `TouchInterceptor` class
used in the Music application &mdash; you use this to re-arrange
a playlist. However, that widget is not in the Android SDK
as of Android 2.2.

`TouchListView` is 99% the Android open source code for
`TouchInterceptor`. `TouchListView` also allows the widget
to be configured from an XML layout file, replacing some
hard-wired values that `TouchInterceptor` uses.

This is available from [the Android Parcel Project](http://andparcel.com) as the
`cwac-touchlist` parcel.

Usage
-----
You will see a sample project that uses `TouchListView` in the
`demo/` directory of the repository. This project is designed
to be built via Ant from the command line. Compile the top-level
project first via `ant parcel`, then compile and install the
demo project via `ant demo`.

To work with `TouchListView` in your own project, install
the `cwac-touchlist` parcel and add a `com.commonsware.cwac.tlv.TouchListView`
widget to your XML layout file. You have five customizable
attributes:

 * `normal_height`: the height of one of your regular rows (required)
 * `expanded_height`: the largest possible height of one of
 your rows (defaults to the value of `normal_height`)
 * `grabber`: the `android:id` value of an icon in your rows
 that should be used as the "grab handle" for the drag-and-drop
 operation (required)
 * `dragndrop_background`: a color to use as the background of your
 row when it is being dragged (defaults to being fully transparent)
 * `remove_mode`: can be none (user cannot remove entries), slide
 (user can remove entries by dragging outside of the list),
 or fling (...not quite sure what this does) (defaults to none)
 
For example, here is the layout from the demo project:

	<?xml version="1.0" encoding="utf-8"?>
	<com.commonsware.cwac.tlv.TouchListView
		xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:tlv="http://schemas.android.com/apk/res/com.commonsware.cwac.tlv.demo"
	
		android:id="@android:id/list"
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:drawSelectorOnTop="false"
		tlv:normal_height="64dip"
		tlv:grabber="@+id/icon"
		tlv:remove_mode="slide"
	/>

You will need to change the `com.commonsware.cwac.tlv.demo` to
your own project's package in the `tlv` namespace declaration.

In code, you set up a `TouchListView` just like a regular
`ListView`, except that you need to register a `TouchListView.DropListener`
via `setDropListener()`. In your listener, you will need to do
something to affect the re-ordering requested via the drag-and-drop
operation. In the demo project, this is a matter of removing
the entry from the old position and putting it in the new position.

If you have remove_mode enabled, you will also need to register a
`TouchListView.RemoveListener` via `setRemoveListener()`. This
will be notified when the user removes an entry from the list.
Once again, you need to make this change permanent in your
data model. In the demo project, this removes the row from the
actual `ArrayAdapter` supporting the list.

There is also a `TouchListView.DragListener` that you can register
via `setDragListener()`, if you want to know when the user
has initiated a drag operation.

Dependencies
------------
This depends upon the `cwac-parcel` parcel for accessing
project-level resources.

Version
-------
This is version 0.1 of this module, meaning it is pretty darn
new, though the underlying Android open source code has been
in use for quite some time.

Demo
----
There is a `demo/` directory containing a demo project. If you
have the Android Parcel Project client installed, you can
run the `ant demo` command to install the requisite parcels into
the demo project and install the resulting APK into your
attached emulator or device. You will need to have compiled
the top-level project via `ant parcel` to set up a local copy
of the parcel file for `ant demo` to use.

License
-------
The code in this project is licensed under the Apache
Software License 2.0, per the terms of the included LICENSE
file.

Questions
---------
If you have questions regarding the use of this code, please
join and ask them on the [cw-android Google Group][gg]. Be sure to
indicate which CWAC module you have questions about.

Bear in mind that the person who converted `TouchInterceptor`
into `TouchListView` does not fully understand the original
`TouchInterceptor` code, and so support may be limited.

[gg]: http://groups.google.com/group/cw-android
