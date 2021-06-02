# Development of a 4k/1080@ 30fps video recording/playback for c610

## dependencies :
   - adb (android debugger bridge)
	   ```
       $ sudo apt-get install android-tools-adb
     ```     
   - vlc palyer 


## To setup cross compile environment:

 - To Install the Application SDK,  download the sdk using  url given in the section 2 of the below document 
  ```
    “[TC_C610LE_23110]_TurboX C610 OK_Application SDK User Manual_V1.0.pdf“.
  ```
    
  - After downloading, unpack the application sdk and  execute the below script file it will ask the default target directory, press enter and type 'Y'
    ```
    /# ./oecore-x86_64-armv7ahf-neon-toolchain-nodistro.0.sh
    ```

   - This complete the environment setup.


## To run this project: 
   
### step1:  Invoke the source environment in host system 
  ```	 	
	$ source /usr/local/oecore-x86_64/environment-setup-armv7ahf-neon-oe-linux-gnueabi
  ```

### step2 :  Compile the source code using $CC
   
   ```	
	 $ $CC video_record.c video_record.h -o video `pkg-config --cflags --libs gstreamer-1.0' 
	 
   ```
   
### step3 : Push the binary to /bin of target device 
	
  ```
	$ adb root
	$ adb remount
	$ adb shell mount -o remount,rw /
	$ adb forward tcp:8900 tcp:8900
	$ adb push video /bin/
	
  ```
  
### step4 : Run the command in target environment 
 	
  ```
	$ adb shell
	
  ```
	
  **case1** :  To run 4k video
  
  ```
  /# video 4k /data/video/4k.mp4
  (ctrl +c to stop)
  
  ```      
	
  **case2** : To run 1080p video
	
  ```
  /# video 1080p /data/video/1080p.mp4
	(ctrl+c to stop)
	
  ```
	Using adb pull command you can download both 4k and 1080p video from target device
	
  ```
  $ adb pull /data/video/4k.mp4
	$ adb pull /data/video/1080p.mp4
	
  ```
	
  **case3** : Tcp live stream 
	
  ```
  /# video tcp 127.0.0.1
	(ctrl +c to stop) 	
	
  ```
	Run following commands on new teriminal on host system to see the tcp streaming

  ```
  $ adb forward tcp:8900 tcp:8900
  $ vlc -vvv tcp://127.0.0.1:8900
	
  ```	 	
