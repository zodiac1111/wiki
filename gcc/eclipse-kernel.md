# 基本配置

参考 [[https://wiki.eclipse.org/HowTo_use_the_CDT_to_navigate_Linux_kernel_source]]

简单内容摘录如下:

1. Download and install Eclipse plus the CDT.
2. Configure and build your kernel to define CONFIG_* and generate autoconf.h. This can be done before or after downloading and installing Eclipse.
3. Ensure that you have the right kernel source (e.g. make sure you are on the right git branch). If you check out another branch later, that's ok, but you will need to re-index the source, and that takes about 20 minutes.
4. Start up Eclipse.
5. Click File->New->C Project
6. Fill in a project name like my_kernel
7. Uncheck the Use default location box and type in the root directory of your kernel into the Location box.
8. In the Project type: pane, click the Makefile project and select Empty Project
9. On the right side, select Linux GCC
10. Click Advanced settings... and a Properties dialog will pop up.
11. Open the C/C++ General selection on the left.
12. Click on Preprocessor Include Paths
13. Select GNU C in the Languages list
14. Select CDT User Setting Entries in the Setting Entries list
15. Click on Add.... Choose Preprocessor Macros File from the top left dropdown, Project Path from the top right dropdown, and enter "include/generated/autoconf.h" into the File text box. (Note: For kernels older than 2.6.33, the location of autoconf.h is include/linux/autoconf.h)
16. Also add any other macros files you are using.
17. Click on Indexer
18. Checkmark the Enable project specific setttings box.
19. Uncheck Index source files not included in the build
20. Click on Paths and Symbols on the left.
21. Select the Includes tab and then select GNU C
22. Click Add...
23. Click Workspace... then select your kernel's include, and include/uapi directories
24. Do another Add, Workspace and add both arch/architecture/include, and arch/architecture/include/uapi directories. e.g., arch/powerpc/include and arch/powerpc/include/uapi (The UAPI directories are due to the kernel's user/kernel header split covered here in-detail)
25. Click the # Symbols tab
26. Click Add...
27. Set the name to __KERNEL__
28. Set the value to 1 and click OK
29. Click the Source Location tab
30. Click the plus sign next to your project name.
31. Select the Filter item and click Edit Filter...
32. Click Add Multiple... and then select all of the arch/* directories in your kernel source that will not be used (i.e. all the ones that are not for the architecture you are using)
33. Click OK and OK again to dismiss that dialog.
34. Under C/C++ General, select Preprocessor Include Paths, Macros etc.
35. Click the Providers tab and select CDT GCC Built-in Compiler Settings
36. Uncheck Use global provider shared between projects
37. Add -nostdinc to the Command to get compiler specs
38. Check Allocate console in the Console View so you can see that this is working
39. Click OK on the Properties dialog.
40. Click Finish on the C Project dialog.
41. The Project will index automatically.
42. On a platter drive indexing will take upwards of 20 minutes to complete, on a SSD indexing will take about 5 minutes to complete.

# 交叉编译

参考 [[http://kloggsays.blogspot.com/2011/06/kernel-development-using-eclipse-omap4.html]]

交叉编译环境可能有些许不同的地方.

* Step 14: since we are doing cross compilation, I need to add custom build variables in the corresponding menu of the C/C++ Build options. Add ARCH with value arm and CROSS_COMPILE with value arm-linux-gnueabi- to all configurations. Maybe I will need to add some compiler options later here, but for now it is quite enough
* Step 15: use arm-linux-gnueabi-gcc for compiler command
* Step 25: here I have arch/arm/include, also I had to add arch/arm/plat-omap/include and arch/arm/mach-omap2/include
* Step 33: here I have everything except arch/arm
Additionally in C/C++ Build options I am setting Build target in the Behavior tab to uImage and Build command in the Builder Settings tab to make ARCH=${ARCH} CROSS_COMPILE=${CROSS_COMPILE}
In the end you will need to clean and rebuild from Eclipse to get the list of issues 
