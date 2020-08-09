[Home Page](https://github.com/TrackerLounge/Home)

[OpenCV SURF](https://github.com/TrackerLounge/OpenCVSURF)

[ReferenceMaterial](https://github.com/TrackerLounge/OpenCVSURF/blob/master/ReferenceMaterial.md)

# Youtube Video
[![Alt text](https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/splashScreen_small.jpg)](https://www.youtube.com/watch?v=TP2QohEGAl4)

# Video Transcript
In this video we will take a look at the process of compiling 
the OpenCV source code into Java.

We will include the OpenCV contrib modules needed to run SURF feature descriptions.

Note: the SURF algorithm may still be copy-righted and should not be used for production purposes unless the copy-right status is understood and addressed.

Our goal is to run the example code shown on the OpenCV web page:
https://docs.opencv.org/4.4.0/d5/dde/tutorial_feature_description.html

I will be working in Java jdk1.8.0_101 and the Eclipse IDE

Here is an example of output from this code.


OpenCV provides pre-compiled java jar and dll files
https://opencv.org/releases/

However, if you use the pre-bundled OpenCV jar files and dll files, you will get errors when working thru the OpenCV Tutorial

Specifically, the org.opencv.xfeatures2d package will not be found and will prevent the code from compiling.

In order to run the tutorial, we will need to download the OpenCV source code from GitHub and the OpenCV_Contrib source code from GitHub and build it locally.

https://github.com/opencv/opencv

https://github.com/opencv/opencv_contrib

I found David Delabassee's webpage helpful in learning this process;
https://delabassee.com/OpenCVJava/

I also found the on-line book - OpenCV 3.0 Computer Vision with Java helpful.
https://subscription.packtpub.com/book/application_development/9781783283972/1/ch01lvl1sec09/building-opencv-from-the-source-code

I created a folder called opt/opencv/workspace.
We will check out the OpenCV and OpenCV_Contrib source code from github to this location.

git clone https://github.com/opencv/opencv.git

git clone https://github.com/opencv/opencv_contrib.git

Ok, we are back and have the downloaded source code.

Next we will create a folder to build our code into.
I will call this "build"

For easy, I will create a bat file to contain the commands to compile the code.
I will call this "build_opencv.bat".

In the build_opencv.bat, I will set the java version.
I want the build and eclipse IDE to use the same version of java.

I have a number of Java instances on my machine. Some programs like Oracle DB comes with a pre-bundled version of Java and that can get in the way in your path.

Your java install location is likely to be different than what I have.

```Content of build_opencv.bat
Set JAVA_HOME= C:\opt\el\java\jdk1.8.0_101

set PATH=C:\opt\el\java\jdk1.8.0_101\bin;%PATH%

java -version
```

If you know you only have one instance of Java installed on your machine, you may not need to set the java version. But it is a good idea to check. 

Open a DOS command prompt and run the commands

If the java version used during the compile does not match the run time java version in Eclipse IDE, you will get an error like this:
java.lang.UnsupportedClassVersionError

In order to compile the C++ code into Java, we will need to download CMAKE and install it on our machine.
https://cmake.org/download/

I did this earlier using he cmake-3.18.1-win64-x64.msi install.

Here are a few screenshots.

I also installed Microsoft Visual Studio Community Edition
https://visualstudio.microsoft.com/downloads/

I used the free download.

Some versions of the instructions used this software.
I am not sure if you need it or not.
It may been needed in the CMAKE path.

Here are some screenshots of the install.

You will need Python 2.7 installed on your machine.
This is an older depricated version which is not ideal but what are you going to do.

You will also need to have numpy installed.
You can get it by running pip install numpy

You can check that it is available by opening a python command line window and runing 
import numpy as np
And verifying that you get no errors

We now have CMAKE and related software installed on the machine.

We can add the CMAKE command to our build_opencv.bat to compile and build the code.

Here is what we start with:
```Content of build_opencv.bat
cmake -S opencv/ -B build/

cmake --build build/
```

This should populate our build folder with the code and then compile it.

However as it, the resulting code will not run.

We will have to add a few flags and attributes.
We will talk about each in turn.

The first error I ran into with this code was:
java.lang.UnsatisfiedLinkError 
Can't load IA 32-bit .dll on a AMD 64-bit platform

I had to add:
-DCMAKE_GENERATOR_PLATFORM=x64

The next error I ran into was:
java.lang.UnsatisfiedLinkError 
Can't find dependent libraries

I had to add the flag:
-DBUILD_SHARED_LIBS=OFF

At this point the command will produce OpenCV core jar and dll files that will do basic OpenCV commands.

However, we have not yet bundled the OpenCV_Contrib code.
As a result the SURF code will not yet run.

We need to tell CMAKE to include opencv_contrib in the build and where to get it from.

We do this by adding an command attribute like:
-DOPENCV_EXTRA_MODULES_PATH=D:/opt/opencv/workspace/opencv_contrib/modules

We should now be able to produce java jar files and dll files that contain the core opencv and opencv_contrib libraries.

However, if you try to run this code as is and call SURF and related patented code you will get an error:
Error: The function/feature is not implemented (This algorithm is patented and is excluded in this configuration: Set OPENCV_ENABLE_NON_FREE CMAKE option and rebuild the library)

To fix this, we will add the command line attribute:
-DOPENCV_ENABLE_NONFREE=ON

Up to this point I have told you the errors you will get.
We haven't actually run the CMAKE command.

Let's do that now.

The contents of build_opencv.bat are now:
```Content of build_opencv.bat
Set JAVA_HOME= C:\opt\el\java\jdk1.8.0_101

set PATH=C:\opt\el\java\jdk1.8.0_101\bin;%PATH%

java -version

cmake -DCMAKE_GENERATOR_PLATFORM=x64 -DBUILD_SHARED_LIBS=OFF -DOPENCV_EXTRA_MODULES_PATH=D:/opt/opencv/workspace/opencv_contrib/modules -DOPENCV_ENABLE_NONFREE=ON -S opencv/ -B build/

cmake --build build/
```

We will open a DOS prompt by typing CMD and then type:
build_opencv.bat and hit enter.

The build takes about 5 minutes to run. 
I will stop recording and come back when it is done.

Ok we are back. The build is complete.
If we scroll up a little bit we can see a python error that I get.
I haven't see an ill effect from this yet.

The build folder is now populated.

We care about two folders:
D:\opt\opencv\workspace\build\bin
This contains the opencv-440.jar

And
D:\opt\opencv\workspace\build\lib\Debug
This contains the opencv_java440.dll

We will need to tell Eclipse about both of these files.

I used instructions at this website to configure Eclipse
https://opencv-java-tutorials.readthedocs.io/en/latest/02-first-java-application-with-opencv.html

It explains how to tell Eclipse about the opencv jar and dll file.

In Eclipse, Click on Window > Preferences
Click on Java > Build Path > User Libraries

I have a couple of instances of OpenCV installed here.
Let's create a new one.

Click New
Give it a name: opencv_youtube
Add External Jars
Browse to:
D:\opt\opencv\workspace\build\bin
Select opencv-440.jar

Click on Native library location
Edit 
Click External Folder
Browse to 
D:\opt\opencv\workspace\build\lib\Debug
Click ok

Click apply and close

Right Click on the project and click Properties
Click on Java Build Path > Libraries
I have a version of opencv already linked.
However, this is the pre-compiled version provided by the OpenCV web page.
I want to replace this with my one locally compiled library.

Click add library
Click User Library
Click Next

Select opencv_youtube and click finish.

I will remove the pre-compiled version of opencv

Click Apply and Close.

We test the code with two different views of the same track 

<img src='https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/LF_20in_Stride_Wet_Sand_Binary_Small.jpg' width=800>

For details on how this image was created see:

[FFT - Fast Fourier Transform and Edge Detection](https://github.com/TrackerLounge/TrackingAndEdgeDetection/blob/master/FastFourierTransformAndEdgeDetection.md)

<img src='https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/LF_20in_Stride_Wet_Sand_Binary_Small_Rotated_and_Scaled.jpg' width=800>

Here are the calculated matching SURF descriptors using the code listed at
[Feature Description](https://docs.opencv.org/4.4.0/d5/dde/tutorial_feature_description.html)

<img src='https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/matches.png' width=800>

If we apply the code from:
[OpenCV Tutorial - Features2D + Homography to find a known object](https://docs.opencv.org/4.4.0/d7/dff/tutorial_feature_homography.html)

We get:

<img src='https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/SURFGoodMatchesAndObjectDetection.jpg' width=800>

Which is an excellent result!

What about on original sand images when the track has good contrast

<img src='https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/track00.jpg' width=800>

<img src='https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/track00_scaled_rotated.jpg' width=800>

We get:

<img src='https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/SURFGoodMatchesAndObjectDetectionOnOriginalSandImage.jpg' width=1800>

Again, these are good results.

If we try to compare a track to a flipped track (simulating left foot comparing to right foot), we get:

<img src='https://github.com/TrackerLounge/OpenCVSURF/blob/master/CompilingOpenCV/NoMatchOnALeftTrackToRightTrackInSand.jpg' width=1800>

No match detected. This is a good result in my mind as it shows that we can tell the difference between right and left foot.



