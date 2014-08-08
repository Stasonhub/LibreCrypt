Technical Details: Building the Software         
				  
				Technical Details: Building the Software 
					DoxBox/DoxBox4PDA/DoxBox Explorer come in a number of parts:  
						DoxBox:  
							A front-end GUI, written in Delphi 
							A number of kernel drivers, written in C  
						DoxBox4PDA:  
							A front-end GUI, written in C 
							A number of drivers, written in C  
						DoxBox Explorer:  
							A front-end GUI, written in Delphi 
							The same drivers as FreeOTFE4PDA, but built for Win32  
						A number of command line decryption utilities, also written in C.    
					DoxBox FreeOTFE4PDA 
						DoxBox Explorer 
							Building the Command Line Decryption Utilities 
							Signing the Binaries 
							Additional Notes     
							DoxBox Building the GUI  
								This is a description for Delphi newbies of the basic steps involved in compiling the DoxBox GUI.  To build the GUI, the following software is required:  
									Delphi (CodeGear Delphi 2007 or later, though Delphi 2006 should just as well. Delphi v5 - v7 can probably be used with minimal changes, though wouldn't look as nice under Windows Vista) 
									The SDeanComponents package (v2.00.00 or later) (Optional) GNU gettext for Delphi (dxgettext), available (free) from: http://dybdahl.dk/dxgettext/ (This package adds support for language translations)  
									The binary release of this software was built with CodeGear Delphi 2007.  
								With each of the packages in the SDeanComponents archive,  
									Build each package 
									Install each package 
										Ensure that the correct path to each package is added to your Delphi environment ("Tools | Environment Options...", "Library" tab)  
										Add the path to the modified Delphi files included in SDeanComponents to fix various bugs relating to Delphi 2006's Windows Vista support to the top of Delphi's standard library paths. (This step probably won't be needed with later versions of Delphi, and shouldn't be carried out with older versions of Delphi, which will have different source)
									Open the DoxBox project ("DoxBox.dpr") 
										If you have the dxgettext software installed (see above), ensure that the compiler directive "_DXGETTEXT" is set. Otherwise, make sure that this compiler directive is not set. 
									Build the application. You should now find a file called "DoxBox.exe" in the directory above the "src" directory  You have now successfully built the GUI frontend!  If required, the compiler definition "FREEOTFE_TIME_CDB_DUMP" may be set, in which case the time taken to dump a CDB ("Tools | Critical data block | Dump to human readable file...") will be shown after the dump completes.  
						Building the Kernel Drivers 
							The kernel mode drivers implement the actual hash, encryption/decryption and main FreeOTFE drivers.  
								To build these drivers, the following software is required:  
								Microsoft Visual Studio 2008 (older versions may well be used, changing "vcvarsall" to "vcvars32", and similar changes)  If using an older version of MS Visual Studio, the MS Windows SDK (February 2003 version) is also needed  
								The MS Windows WDK (WDK for Server 2008 v6001.18001)  
								The binary release of this software was built with Microsoft Visual Studio 2005 Professional Edition.  At time of writing, the MS Windows SDK can be downloaded from the Microsoft WWW site. The MS Windows DDK is not available as a download, but can be ordered from the Microsoft WWW site as a free CD, for the cost of delivery.  It should be noted that if you are unable to source the exact versions listed above, earlier versions may well be substituted, although I cannot guarantee success. Later versions should operate correctly. The above list describes the development environment as used to build the binary release of DoxBox.
								Setting up the Build Environment Installation and Configuration of MS Build Environment 
								The following list comprehensively describes the configuration used to build the binary release of DoxBox. Feel free to adjust according to taste - a number of the options listed are not necessary, and are only included for completeness...  
								Install VC++ Put a copy of "vcvarsall.bat" into one of the directories in your path 
								Configure the VC++ editor:  To use spaces, not tabs To indent braces  
								Install the MS Windows SDK with the following options:  
									Install in C:\MSSDK 
									Install the "Core SDK" 
									Then install the debugging tools for windows 
									Do not register environment variables (we'll use "Setenv.bat" from the command line)  
								Install the MS Windows DDK with the following options:  
									Install in C:\WINDDK\3790 Include the "Illustrative Driver Samples" 
									Include the "Input Samples" Include the "Storage Samples"
									Include the "Virtual Device Driver Samples" 
									Include the "WDM Samples" Build Environment\Windows Driver Development Kit AMD64 Additional Build Tools Build Environment\Windows Server 2003 AMD64 Libraries Build Environment\Windows XP Headers Build Environment\Windows XP x86 Libraries Build Environment\Windows XP IA86 Libraries  Needed if the build .BAT files (see later) use "chk WXP" - if that's skipped it'll default to WNET (windows .NET)  
									Build Environment\Windows 2000 Headers Build Environment\Windows 2000 Build Environment 
									Needed if the build .BAT files (see later) use "chk <something>" - if that's skipped it'll default to WNET (windows .net)    
								DoxBox Build Configuration  
									Edit "setup_env_common.bat" (located under src\drivers\Common\bin), and ensure that the following variables are set appropriately:    
									Variable  				Description  																								Default value   
									FREEOTFE_DEBUG|	 Build type flag; set to 1 for debug build, or 0 for release|	 0|	  
									FREEOTFE_TARGET|	 Target OS to build for; e.g. WXP/W2K/WNET; note that W2K builds will not operate correctly under Windows XP (e.g. when formatting a volume)|	 WXP|	  
									PROJECT_DRIVE|	 The drive on which you have stored the FreeOTFE source|	 E:|	  
									PROJECT_DIR|	 The full drive and path where the "drivers" directory is located|	 <see file>|	  
									MSSDK_DIR|	 The directory in which you installed the MS SDK|	 C:\MSSDK|	  
									MSDDK_DIR|	 The directory in which you installed the MS DDK|	 C:\WINDDK\3790|	    

									Edit "setup_env_driver.bat" (in the same directory), and ensure that "SETENV.BAT" is called with the parameters appropriate to the type of build you wish to create, and that "FREEOTFE_OUTPUT_DIR" is set to the appropriate directory under the source directories where the build executable places the files it creates (this shouldn't be needed as it will happen automatically if the above are configured correctly)  3rd Party Source Code Some of the FreeOTFE drivers (the hash/encryptions drivers in particular) are dependant on certain 3rd party software being installed. DoxBox's source code comes complete with 3rd party included in the"src\3rd_party" directory and should be preconfigured, ready for use. 
									Alternatively, you may wish to download this 3rd party source from the original authors in order to verify the integrity of this software. For this reason, details of where this software was obtained from are included in the above directory. 
									Please note that should choose the latter option, it is important that you review the individual driver notes (see separate driver directories; "_notes.txt" files) to ensure that this software is configured correctly. Additionally, you may well have to modify the "my_build_sys.bat" files, directing them to the location where you installed said 3rd party source code, as the build process requires that certain files are copied over into the DoxBox src directories. (Annoying, but this is a requirement of the MS "build.exe" command) 
									The LibTomCrypt source in particular had minor configuration changes to tomcrypt_cfg.h and tomcrypt_custom.h; please compare the original source (a copy of its release ZIP file is stored under src\3rd_party\libtomcrypt) with the modified version (uncompressed in a directory under this one)
								Building the FreeOTFE Drivers Either:
									Open "DoxBox.sln" using Visual C++ 
									Rightclick on each project in turn, and select "Build"  
								or:
									Enter each of the separate driver directories in turn and launch each project's "my_build_sys.bat"  In either case, a copy of the binary which is built will be copied into the directory above your "src" directory. 
								After reaching this stage, you should have successfully built your own version of the FreeOTFE drivers! 
Notes:
	If FREEOTFE_TARGET is set to W2K, the resulting binary may not operate correctly under MS Windows XP as a number of functions what are only needed under Windows XP and later are #ifdef'd out. As a result, a "W2K" binary may not operate correctly under Windows XP (e.g. trying to format a volume may result in... Nothing happening). If you want a binary which will operate under both Windows 2000 and Windows XP, set this to WXP. 
	Windows XP migrated a couple of the previous Windows 2000 macros to be functions. In order to allow the above "WXP" builds to work under Windows 2000, "IFSRelated.h" includes a copy of these macros, and uses them regardless - see comments in code for an explanation.  
 
  FreeOTFE4PDA To build FreeOTFE4PDA (both the drivers and GUI):  
  	Open "FreeOTFE4PDA.sln" using Visual C++ 
  	Set the build configuration within Visual C++ to "Release" - "Pocket PC 2003 (ARM4)" 
  	Rightclick on each project in turn, and select "Rebuild"  A copy of the binary which is built will be copied into the directory above your "src" directory. 
 
  DoxBox Explorer Building the GUI 
  	This is a description for Delphi newbies of the basic steps involved in compiling the DoxBox Explorer GUI. 
  	To build the GUI, the following software is required:
  	Delphi (CodeGear Delphi 2007 or later, though Delphi 2006 should just as well. Delphi v5 - v7 can probably be used with minimal changes, though wouldn't look as nice under Windows Vista) 
  	The SDeanComponents package (v2.00.00 or later) 
  	(Optional) GNU gettext for Delphi (dxgettext), available (free) from: http://dybdahl.dk/dxgettext/ (This package adds support for language translations)  The binary release of this software was built with CodeGear Delphi 2007.
  	With each of the packages in the SDeanComponents archive,  
			Build each package 
			Install each package 
			Ensure that the correct path to each package is added to your Delphi environment ("Tools | Environment Options...", "Library" tab)  
			Add the path to the modified Delphi files included in SDeanComponents to fix various bugs relating to Delphi 2006's Windows Vista support to the top of Delphi's standard library paths. (This step probably won't be needed with later versions of Delphi, and shouldn't be carried out with older versions of Delphi, which will have different source)
		Open the DoxBox Explorer project ("DoxBoxExplorer.dpr") 
			If you have the dxgettext software installed (see above), ensure that the compiler directive "_DXGETTEXT" is set. Otherwise, make sure that this compiler directive is not set. 
		Build the application. 
			You should now find a file called "DoxBoxExplorer.exe" in the directory above the "src" directory  You have now successfully built the GUI frontend!
			Building the DLL drivers To build the DLLs used by DoxBox Explorer:  
			Open "FreeOTFE4PDA.sln" using Visual C++ 
			Set the build configuration within Visual C++ to "Release" - "Win32" 
			Rightclick on each project in turn, and select "Rebuild". Note: Don't bother building the "GUI" project; at present, this can only be built for the Windows Mobile platform.  A copy of the binary which is built will be copied into the directory above your "src" directory. 
 
   Building the Command Line Decryption Utilities Note: The development of the command line decryption utilities has ceased. This functionality has been superceded with the development of DoxBox Explorer 
   	To build the command line decryption utilities, the following software is required:
   	A C compiler (Visual C++ .NET was used to write and test this software)  Please follow the following steps:
  Install and configure up the build environment, as described as per building the backend drivers, you may omit the SDK and DDK. 
  Modify the software as appropriate for your test  
  Please see the command line decryption utility documentation  
  Launch the relevant "my_build_exe.bat" file  The executable should be built in the same directory. 
 
  Signing the Binaries
  	the drivers need to be signed even if DISABLE_INTEGRITY_CHECKS is on.
		To sign the DoxBox binary files (.exe, .dll and .sys files), the procedure is pretty much as described at: Pantaray Research WWW site
		At present, DoxBox is signed using a self-signed certificate; the full procedure used is as follows:  
			Install Visual Studio 
			From a command prompt, run "vcvarsall" (all commands detailed below should be executed from this command prompt) 
			Create a private certificate:
				code|
					makecert.exe -sv sdean12.pvk -n "E=sdean12@sdean12.org,CN=Sarah Dean" sdean12.cer
					makecert.exe -r -pe -sv tdk.pvk -n "E=doxbox.tdk@squte.com,CN=tdk" tdk.cer
				see http://msdn.microsoft.com/en-gb/library/windows/hardware/ff548693(v=vs.85).aspx
				this will prompt for private key password 3 times 
				this should create two files: tdk.pvk and tdk.cer 
			Create a test software publisher certificate (SPC):  
				code|
					cert2spc.exe tdk.cer tdk.spc
				to create tdk.spc. (This file would normally be supplied by a CA, if purchased) 
			Create a personal information file:  
				code|
					pvk2pfx -pvk sdean12.pvk -spc sdean12.spc -pfx sdean12.pfx -f /pi <pvk password> /po <pfx password>
					pvk2pfx -pvk tdk.pvk     -spc tdk.spc     -pfx tdk.pfx     -f /pi <pvk password> /po <pfx password>
				Where:  
					<pvk password> is the password used when generating the .pvk file with makecert.txt 
					<pfx password> is the password you wish to use for securing the new .pfx file  
			Sign each of binary files:  
				code|
					signtool.exe sign /f tdk.pfx /p <pfx password> /v /t http://timestamp.verisign.com/scripts/timstamp.dll <filename>
				or run sign_all.bat to sign all of them  
			Where:  
				<pfx password> is the password used when generating the .pfx file with pvk2pfx  
			The URL specified is a time stamping service (Verisign's in this case).  
	Additional Notes 
		When building the C code, FreeOTFEPlatform.h automatically #defines one of the following:  
		FreeOTFE_PC_DRIVER 
		FreeOTFE_PC_DLL 
		FreeOTFE_PDA  
		depending on what is being built. 
		This header file should be #included at the start of every file which uses any of these defines. (Yes, this is obvious - but easily overlooked!)
					