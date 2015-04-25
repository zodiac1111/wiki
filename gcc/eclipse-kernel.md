参考 [[https://wiki.eclipse.org/HowTo_use_the_CDT_to_navigate_Linux_kernel_source]]

简单内容摘录如下:
```
Disclaimer: these steps were last updated for Eclipse Juno 4.2.2 + CDT 8.1.2, and originally developed for Eclipse 3.5.1 + CDT 6.0.0.

Download and install Eclipse plus the CDT.
Configure and build your kernel to define CONFIG_* and generate autoconf.h. This can be done before or after downloading and installing Eclipse.
Ensure that you have the right kernel source (e.g. make sure you are on the right git branch). If you check out another branch later, that's ok, but you will need to re-index the source, and that takes about 20 minutes.
Start up Eclipse.
Click File->New->C Project
Fill in a project name like my_kernel
Uncheck the Use default location box and type in the root directory of your kernel into the Location box.
In the Project type: pane, click the Makefile project and select Empty Project
On the right side, select Linux GCC
Click Advanced settings... and a Properties dialog will pop up.
Open the C/C++ General selection on the left.
Click on Preprocessor Include Paths
Select GNU C in the Languages list
Select CDT User Setting Entries in the Setting Entries list
Click on Add.... Choose Preprocessor Macros File from the top left dropdown, Project Path from the top right dropdown, and enter "include/generated/autoconf.h" into the File text box. (Note: For kernels older than 2.6.33, the location of autoconf.h is include/linux/autoconf.h)
Also add any other macros files you are using.
Click on Indexer
Checkmark the Enable project specific setttings box.
Uncheck Index source files not included in the build
Click on Paths and Symbols on the left.
Select the Includes tab and then select GNU C
Click Add...
Click Workspace... then select your kernel's include, and include/uapi directories
Do another Add, Workspace and add both arch/architecture/include, and arch/architecture/include/uapi directories. e.g., arch/powerpc/include and arch/powerpc/include/uapi (The UAPI directories are due to the kernel's user/kernel header split covered here in-detail)
Click the # Symbols tab
Click Add...
Set the name to __KERNEL__
Set the value to 1 and click OK
Click the Source Location tab
Click the plus sign next to your project name.
Select the Filter item and click Edit Filter...
Click Add Multiple... and then select all of the arch/* directories in your kernel source that will not be used (i.e. all the ones that are not for the architecture you are using)
Click OK and OK again to dismiss that dialog.
Under C/C++ General, select Preprocessor Include Paths, Macros etc.
Click the Providers tab and select CDT GCC Built-in Compiler Settings
Uncheck Use global provider shared between projects
Add -nostdinc to the Command to get compiler specs
Check Allocate console in the Console View so you can see that this is working
Click OK on the Properties dialog.
Click Finish on the C Project dialog.
The Project will index automatically.
On a platter drive indexing will take upwards of 20 minutes to complete, on a SSD indexing will take about 5 minutes to complete.
Notes:

Adding include and arch/architecture/include only gets you a couple of the common include paths. To fully index all of the kernel, you would have to add dozens of paths, unfortunately. For this reason, I advise against using PTP's remote indexing capability for the linux kernel, because what happens is that it will report thousands of errors in locating header files, and the process of reporting those errors over a possibly long-latency link, will cause the indexing to take many hours.
If you change any of your CONFIG_* settings, in order for Eclipse to recognize those changes, you may need to do a "build" from within Eclipse. Note, this does not mean to re-build the index; this means to build the kernel, by having Eclipse invoke make (this is normally bound to the Ctrl-B key in Eclipse). Eclipse should automatically detect changes to include/generated/autoconf.h, reread the compilation #defines it uses, and reindex.
The background color of "Quick Context View" will be dark if the Ambiance theme in Ubuntu is selected.
For some people, Eclipse may fail to index the kernel with a out of memory error. The fix seems to be to start eclipse with the arguments: eclipse -vmargs -Xmx650M
```
