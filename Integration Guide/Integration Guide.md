# Integration Guide

## Step 1.
Copy DotzuX.framework to the root directory of your project in Finder.

![](https://raw.githubusercontent.com/DotzuX/DotzuX/master/Integration%20Guide/1.png)

> Note: If integrated by Carthage, don't need to do this. Just check here:<br>
> `YourProjectFolder/Carthage/Build/iOS/DotzuX.framework`
	
## Step 2.
Select the Build Settings tab and add the following to the Debug configuration of the Framework Search Paths (`FRAMEWORK_SEARCH_PATHS`) setting:

`$(inherited) $(SRCROOT)`
	
![](https://raw.githubusercontent.com/DotzuX/DotzuX/master/Integration%20Guide/2.png)

> Note: If integrated by Carthage, use this instead:<br>
> `$(inherited) $(SRCROOT)/Carthage/Build/iOS`

## Step 3.
Still in the Build Settings tab, add the following to the Debug configuration of the Other Linker Flags (`OTHER_LDFLAGS`) setting:

`-ObjC -weak_framework DotzuX`
	
![](https://raw.githubusercontent.com/DotzuX/DotzuX/master/Integration%20Guide/3.png)

## Step 4.
Still in the Build Settings tab, add the following to the Debug configuration of the Runpath Search Paths (`LD_RUNPATH_SEARCH_PATHS`) if it is not already present:

`$(inherited) @executable_path/Frameworks`
	
![](https://raw.githubusercontent.com/DotzuX/DotzuX/master/Integration%20Guide/4.png)

## Step 5.
Select the Build Phases tab and add a new Run Script phase. Paste in the following shell script:

    export DotzuX_FILENAME="DotzuX.framework"
    export DotzuX_PATH="${SRCROOT}/${DotzuX_FILENAME}"
	
    [ "${CONFIGURATION}" != "Debug" ] && exit 0
	
    if [ -d "${DotzuX_PATH}" ]; then
    "${DotzuX_PATH}/copy_and_codesign.sh"
    fi
	
![](https://raw.githubusercontent.com/DotzuX/DotzuX/master/Integration%20Guide/5.png)
	
> Note: If integrated by Carthage, use this instead in line 2:<br>
> `export DotzuX_PATH="${SRCROOT}/Carthage/Build/iOS/${DotzuX_FILENAME}"`