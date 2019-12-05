# KinectSkeletonTracking_POE2019

**This repository supplements the RealSteel RockEmSockEm project at Olin College of Engineering for the Principles of Engineering course in fall 2019**

I spent hours finding resources on getting a kinect to work on ubuntu 18.04 with skeleton mapping. The next steps are to acquire coordinat positions for the joints provided by the kinect, which will be fed into a kinematics solver to map human movements to our little toy robot. 

This project is an immersive learning experience that has the **goal** of pitting two robot torsos against each other in a boxing match. 

Of course this skeleton mapping and kinect usage can be mapped to a lot of other projects, I have made this resource public so that others may use the installation for whatever else they desire. I also did this on a persistent live boot drive and it runs pretty well. 

## **Prereqs (What worked for me)**

- Ubuntu 18.04
- OpenNI 1.5.2
- Sensor Kinect (Avin2 SensorKinect drivers)
- dependencies

## Step 1 - Dependencies

Let's start by making sure you have the dependencies to run the software.
`sudo apt-get install git python libusb-1.0-0-dev freeglut3-dev openjdk-7-jdk`

- git - necessary to access github libraries
- python - necessary to run contents of library that contain python
- libusb - a C library that provides generic access to USB devices (version agnostic)
- freeglut3 - open source alternative to GLUT to complete chores like creating windows, initializing OpenGL contexts, handling input events, etc
- openjdk-7-jdk - java development kit which serves as a compiler for building java applications

`sudo apt-get install doxygen graphviz mono-complete`

## Step 2 - OpenNI

Now that the dependencies are out of the way, let's get OpenNI installed. 
{
    mkdir ~/kinect
    cd ~/kinect
    git clone https://github.com/OpenNI/OpenNI.git
   }
  
Run RedistMaker to install output binaries. I could not install RedistMaker so I've reported the route that got my OpenNI setup to install properly. 

{
    cd OpenNI/Platform/Linux/CreateRedist/
    chmod +x RedistMaker
    ./RedistMaker
    cd ../Redist/OpenNI-Bin-Dev-Linux-x64-v1.5.2.23/
    sudo ./install.sh
    }

### Oh no, the Build Failed
I ended up getting this error... If you didn't get an error, then move on to **Step 4**, don't worry about how I struggled. :(
{
    failed to execute: make -j24 -C /home/<user>/Kinect/OpenNI/Platform/Linux
    }

and it said Building Failed! at the end of it or something like that and I got really frustrated. 

But I found a solution at the bottom of [this thread!](https://github.com/OpenNI/OpenNI/issues/26)

I reverted to gcc-4.8 and g++-4.8 and that resolved the error build failure. You can have multiple gcc versions installed and I just happened to arbitrarily choose 6 for testing. I found out how to do this on some [forum.](https://askubuntu.com/questions/26498/how-to-choose-the-default-gcc-and-g-version)

{
    sudo update-alternatives --remove-all gcc 
    sudo update-alternatives --remove-all g++
    sudo apt-get install gcc-4.8 gcc-6 g++-4.8 g++-6
    }
   
Now that the versions are installed, the next thing I did was set them up to be alternatives. The 10 and 20 just serve as priority indicators for the alternatives. I also installed symbol links for cc and c++ to link to gcc and g++ respectively. 
 
 {
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 10
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 20

    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 10
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-6 20

    sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
    sudo update-alternatives --set cc /usr/bin/gcc

    sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
    sudo update-alternatives --set c++ /usr/bin/g++
}
   
Next I just chose the alternatives with the intuitive description provided by the GUI. It will allow you to select the gcc and g++ version using numbers. 

{
    sudo update-alternatives --config gcc
    sudo update-alternatives --config g++
    }

And then I was able to install OpenNI!

## Step 3: Clone Drivers
These are the drivers for the kinect that will enable you to use skeleton tracking. I heard from a source that the original drivers don't give skeleton tracking, unless you don't really care about skeleton tracking (but this is the way I did it so I don't care if you don't care about what I needed for my project).

The OpenNI device driver also won't let you talk to the kinect, so avoid getting the resource through their site. I've made the repo very nicely to have everything you'd need right here, given that you are in my position. 

 {
    cd ~/kinect/
    git clone git://github.com/avin2/SensorKinect.git
    }

This also happens to have a RedistMaker script in a very similar Platform/Linux directory. Hmm, coincidence? I think not! These were made to be integrated with one another!

{
    cd SensorKinect/Platform/Linux/CreateRedist/
    chmod +x RedistMaker
    ./RedistMaker
    cd ../Redist/Sensor-Bin-Linux-x64-v5.1.0.25/
    chmod +x install.sh
    sudo ./install.sh
    }

## Step 4: NITE Installation (1.5.2.23)
Now I needed NITE and you can get that from [openni.org](openni). Just go ahead and search for and download NITE 1.5.2 and you'll be all set.

Haha, so openni.org got bought by apple in 2014 and I thought my life was over. But no, I found this [github repo](https://github.com/arnaud-ramey/NITE-Bin-Dev-Linux-v1.5.2.23) and dropped the file here just to make it a little bit easier for ~~me~~ you. 

Download the archive and extract it so you can go to the Data directory. 

{
    cd ~/kinect
    tar -xvjpf OpenNI-Linux-x64-2.2.0.33.tar.bz2
    cd NITE-Bin-Dev-Linux-x64-v1.5.2.21/Data
    }

Then I went back to the NITE directory and ran the install script. 

{
    cd ..
    sudo ./install.sh
    }

And then that was it. I got the majority of this information from this awesome [link](http://mitchtech.net/ubuntu-kinect-openni-primesense/) with a little bit of web surfing to makeup for the 7 year gap in technology. I'd recommend going back to the [link](http://mitchtech.net/ubuntu-kinect-openni-primesense/) if I made any mistakes, but I hope I was clear. 

If I go to either of the two directories, I'm able to run all of the same programs which almost all start with Sample-Ni and the camera and kinect sensors initiate. Just type them exactly as listed. 

{
    NiViewer
    Sample-NiAudioSample
    Sample-NiBackRecorder
    Sample-NiCRead
    Sample-NiConvertXToONI
    Sample-NiHandTracker
    Sample-NiRecordSynthetic
    Sample-NiSimpleCreate
    Sample-NiSimpleRead
    Sample-NiSimpleSkeleton
    Sample-NiSimpleViewer
    Sample-NiUserSelection
    Sample-NiUserTracker
{
    ~/kinect/OpenNI/Platform/Linux/Bin/x64-Release
    }
   
**OR**

{
    ~kinect//NITE-Bin-Dev-Linux-v1.5.2.23/x64/Samples/Bin/x64-Release
    }
   










