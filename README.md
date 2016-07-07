# Apple Radars
This repo contains a collection of projects that demonstrate iOS bugs and are referenced in Apple radars.

##1. ImagePicker_PresentationBug
Summary:
When presenting a full screen view controller from a UIImagePickerController, it’s possible for the captured photo to be released after dismissing the presented view controller.

Steps to Reproduce:
See the ImagePicker_PresentationBug test app attached or located at: https://github.com/dustinlange/radars.git
Launch the project and place a breakpoint at line 34 in ViewController.swift

1. Launch the camera from the ‘Launch Camera’ button.
2. Tap the shutter to take a photo.  Do NOT select ‘Use Photo’
3. Place the app in the background by pressing the Home button.
4. Launch the app again.
5. Another view controller will be presented using the UIModalPresentationStyleFullScreen style
6. Tap the ‘Login’ button to dismiss the view controller.
7. The UIImagePickerController is visible now but no longer displaying the captured photo.
8. Tap ‘Use Photo’
9. The breakpoint will be hit because UIImagePickerControllerOriginalImage doesn’t exist in the info dictionary

I understand that UIModalPresentationStyleFullScreen removes views belonging to the presenting view controller but UIImagePickerController doesn’t resume to it’s previous state after dismissing the full screen view controller.  Additionally, UIModalPresentationStyleOverFullScreen behaves as it should but unfortunately I need the view lifecycle functions to be called upon presenting and dismissing.

Expected Results:
UIImagePickerController resumes to it’s previous state with the captured photo visible and ability to handle the photo.  When the user selects the ‘Use Photo’ button, didFinishPickingMediaWithInfo returns the original image.

Actual Results:
The photo preview is completely removed, showing just the ‘Cancel’ and ‘Use Photo’ buttons.  Once the user selects the ‘Use Photo’ button, didFinishPickingMediaWithInfo: doesn’t include the original image.

Regression:
iOS 9.x

Notes:
See the ImagePicker_PresentationBug test app attached or located at: https://github.com/dustinlange/radars.git
