### In order to preserve user trust and maintain data integrity, developing secure mobile application is one of the major challenge for most of the mobile app developers. This article will take you through some of the best practices that should be followed while building Android app to avoid security vulnerabilities.

## Maintain secure communication with other apps
### A. 
<h2> Use Implicit Intents</h2> to show app chooser that provides option to user to launch at least two possible apps on the device for the requested action. This allows users to transfer sensitive information to the app that they trust.

### B. Apply signature-based permissions
while sharing data between two apps that is controlled by you. These permissions do not need user confirmation, but instead it checks that the apps accessing the data are signed using the same signing key. Hence offer more streamlined and secure user experience.

<h2><manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
    <permission android:name="my_custom_permission_name"
                android:protectionLevel="signature" /></h2>

                
