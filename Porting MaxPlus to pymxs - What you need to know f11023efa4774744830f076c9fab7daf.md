# Porting MaxPlus to pymxs - What you need to know

## Introduction

Scripting in 3ds Max has been a way to automate or build custom plugins that either extends the functionality of what 3ds Max offers or introduces new functionality that 3ds Max does not have. Looking at this, its at the core of Design Automation to start running activities in the Autodesk Platform Services (APS) [Formerly Forge]. 3ds Max has been providing scripting capabilities through either the 3ds Max scripting language MAXScript or through Python (a wrapper around the MAXScript). Additionally, 3ds Max offers SDKs for C++ and C# plugin development useful for building and extending capabilities in 3ds Max.

## Scripting in MAXScript

MAXScript comes baked into 3ds Max by default and is accessible o the bottom left window of the UI or through the Scripting menu option in 3ds Max.

![Scripting menu option in 3ds Max 2023](https://raw.githubusercontent.com/allankkoech/mne-blogs/master/img/maxplus2pymxs/Untitled.png)

Scripting menu option in 3ds Max 2023

For instance, to launch the scripting window, click the Scripting Listener option on the drop down above. This brings a Scripting listener window with two options of MAXScript and Python. In the editor window, scripts can be inputted and executed when we move to a new line as shown below.

![MAXScript listener window showing script execution](https://raw.githubusercontent.com/allankkoech/mne-blogs/master/img/maxplus2pymxs/Untitled%201.png)

MAXScript listener window showing script execution

To learn more on how to use MAXScript and its full tools, look at the documentation here [https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=GUID-F039181A-C072-4469-A329-AE60FF7535E7](https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=GUID-F039181A-C072-4469-A329-AE60FF7535E7).

## Scripting in Python

Scripting in python has been an option for quite some time now. Initially, 3ds Max exposed its APIs through a library called MaxPlus ([https://help.autodesk.com/view/3DSMAX/2019/ENU/?guid=__developer_maxplus_python_api_introduction_maxplus_python_api_main_classes_html](https://help.autodesk.com/view/3DSMAX/2019/ENU/?guid=__developer_maxplus_python_api_introduction_maxplus_python_api_main_classes_html)) which became deprecated in 3ds Max 2019 and was replaced by pymxs. Both libraries work differently and have differences in functionalities that get exposed to python.

To compare difference, the code below shows how the structure of code looks like to create an object on scene and modify its parameters.
<br/>

```python  

import MaxPlus
 
obj = MaxPlus.Factory.CreateGeomObject(MaxPlus.ClassIds.Box)
obj.ParameterBlock.Width.Value = 10.0
obj.ParameterBlock.Length.Value = 10.0
obj.ParameterBlock.Height.Value = 10.0  
```
  
```python
import pymxs

obj = pymxs.runtime.box(width=10.0, length=10.0, height=10.0)

```

### Why the difference?

If you are familiar with 3ds Max SDK then you’ll notice the MaxPlus code has a similar approach when creating its objects. It’s basically generated from the C++ SDK and so its approach of code follows how its done in C++ or in .NET. 

Unlike MaxPlus, pymxs on the other hand follows a different approach. Notice the first code we did for MAXScript to add a box to scene, similar - right? Yes, pymxs is built as a wrapper around MAXScript rather than the C++ SDK. As such, the code looks simpler and easy to understand if you know bits of scripting in MAXScript.

## Porting MaxPlus to pymxs

Lets say you have some old scripts you made in MaxPlus and you want to port it over to the recent version of 3ds Max, but now, latest 3ds Max versions no longer ship MaxPlus libraries. The options at hand is porting the codebase to either C++/.NET, MAXScript or python. Lets look at some examples:

### 1. XREFS

xrefs is a terminology for external referencing other files or scenes in the current scene. This prevents loading whole external projects into the current project but rather just referencing it. MaxPlus provides direct manipulation of xref files as follows:

***File counting***  
  
```
import MaxPlus

num_xrefs = MaxPlus.XRefs.GetXRefFileCount()
print(num_xrefs)

```

An equivalent pymxs code for a similar functionality is given below
  
```python  
import pymxs  
num_xrefs = pymxs.runtime.xrefs.getXRefFileCount()  
print(num_xrefs)  
```
  
 ***Getting xref file***

Once xrefs are created within a scene, retrieving specific xref files for any purpose can be achieved as follows in both MaxPlus and pymxs
  
```python  
import MaxPlus

index = 0 # index of the xref file
xref_file = MaxPlus.XRefs.GetXRefFile(index)
print(xref_file)

# IN pymxs

import pymxs

index = 0 # index of the xref file
xref_file = pymxs.runtime.xrefs.getXRefFile(index)
print(xref_file)  
```
  
Note that both requires a parameter specifying the index of the XRef file to get information about.

 ***Setting xref file***

This is intended for setting a file to xref

```python  
import MaxPlus

# get the XRef object that you want to set the file path for
xref_object = MaxPlus.Core.GetINodeByName("MyXRefObject")

# set the file path for the XRef object
file_path = "C:/path/to/my/xref/file.max"
MaxPlus.Core.SetXRefFile(xref_object, file_path)

```  
  
Note that this code represents a workaround for updating the XRef file, since there is no direct way to update the file in pymxs.

```python  
import pymxs

index = 1
# Get the XRef object for index 1
xref_obj = pymxs.runtime.xrefs.getXRefFile(index)

new_file = "path\to\file\something.max"

# Using MAXScript in pymxs
maxscript_code = f'''
axref = xrefs.getXRefFile {index}
axref.filename = "{new_file}"
flagChanged axref
'''

_axref = pymxs.runtime.execute(maxscript_code)

print(_axref)
```  
  
***Flagging xref file as changed***

Used to flag that the xref has been changed.

```python  
import MaxPlus

# get the XRef object that you want to flag as changed
xref_object = MaxPlus.Core.GetINodeByName("MyXRefObject")

# flag the XRef object as changed
MaxPlus.Core.FlagXrefChanged(xref_object)
```  
  
To flag specific file as changed, pymxs doesn’t expose any way to directly do that. Alternatively, we can invoke MAXScript to flag the files as changed as shown;

```python  
import pymxs

index = 1 # Index for the xref you want to flag as updated

maxscript_code = f'''
axref = xrefs.getXRefFile {index}
flagChanged axref
axref
'''

_axref = pymxs.runtime.execute(maxscript_code)

print(_axref)
```  
  
***Updating changed xrefs***

Updates the XRefs that have been flagged as changed. This function actually loads the new versions of the referenced files and updates the XRefs in the scene accordingly.

```python  
# In MAXPLUS
import MaxPlus

# update all XRef objects that have been flagged as changed
MaxPlus.Core.UpdateChangedXRefs()

# In PYMXS
import pymxs

# Updates the changed XRefs and resets their changed flags
pymxs.runtime.xrefs.updateChangedXRefs()
```  
  
### 3. Animation

***Getting Animation Range***

Getting Animation Range is used to retrieve the animation range of the current scene.
  
```python  
import MaxPlus

# get the animation range of the current scene
start_time, end_time = MaxPlus.Animation.GetAnimRange()

# print the start and end times of the animation range
print("Animation Range:")
print("Start Time:", start_time)
print("End Time:", end_time)
```  
  
MaxPlus's corresponding function in pymxs returns  a pair of *time* objects: the first element of the tuple is the animation range's start time, and the second element is the animation range's end time.

```python  
import pymxs

# Get the animation range in 3ds Max
anim_range = pymxs.runtime.animationRange

# Print the start and end times of the animation range
print("Start Time:", anim_range.start)
print("End Time:", anim_range.end)
```  
  
***Getting current time***

Returns current time in the animation

```python  
import MaxPlus

# get the current time in the animation
current_time = MaxPlus.Animation.GetTime()

# print the current time to the console
print("Current Time:", current_time)
```  
  
To get current time when animating in pymxs, the following code samples is used.
  
```python  
import pymxs

# get current time 
current_time = pymxs.runtime.currentTime

# Log the values
print(current_time)
print(current_time.frame)
print(current_time.ticks)
```  
  
## Conclusion

The samples do not cover all the aspects of porting code from MaxPlus to pymxs. However, the full guide can be found on the documentation at [https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=MAXDEV_Python_using_pymxs_maxplus_to_pymxs_html](https://help.autodesk.com/view/MAXDEV/2023/ENU/?guid=MAXDEV_Python_using_pymxs_maxplus_to_pymxs_html). The guide aimed to give ideas on how to look at porting current codes to pymxs. 

We have looked at both pymxs native functions similar to or representing the MaxPlus functions as well as embedding MAXScript in pymxs to achieve functionality that is not currently exposed to pymxs.

## FAQ’s

- Is MaxPlus and pymxs one-to-one match?
    
    No. Due to the architecture, these two bear different exposure to underlying functionalities. So, some functions and modules in MaxPlus are not available in pymxs and vice versa.
    
- Can I convert my MaxPlus code to pymxs fully?
    
    Yes. MaxPlus code can be ported to pymxs but there is no one-to-one conversion. Note that for some functionality, you’ll may need to combine pymxs and MAXScript together to achieve it. It’s all about getting similar functionality.
    
- So, what about pymxs and MAXScript?
    
    pymxs is a MAXScript wrapper but not all functionality and properties in MAXScript is directly generated into pymxs modules/functions/properties. There are work arounds but not a direct function/modules like the ones in MAXScript.