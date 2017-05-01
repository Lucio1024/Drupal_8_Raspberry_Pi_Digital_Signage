# Drupa-Digital-Signage
This Tutorial will guide you into setting up a Drupal website as digital signage, and will  guide you into setting up the Raspberry pi as an intercepter to pull digital content. The raspberry pi can then be be used in any environment… lobby reception, student lounges, cafeterias, break rooms, call centers, elevator banks, manufacturing floors, meeting rooms…virtually any public space or network To Display Digital Content

  Steps
  
      1.Set up a developer environment 
      2.Configure site
      3.Install Drupal modules
      4.Raspberry Pi Digital Signage
      5.Trouble shoot 
  
  #1. Set up a developer environment
  
      To set up a Developer environment on the mac follow the guide below
      
    https://www.drupal.org/docs/develop/local-server-setup/mac-os-development-environment/howto-create-a-local-environment
  
  #2. Configure Site 
  
      1. Add Toxonomy Vocabulary 
  
          Campus
          public (space)
       
      2. Add toxomomy terms (ie.)
          building
          Loby
          Call Center
          Elevetors
 
#3. Install Drupal Modules

      1. On terminal:
          $ cd sites/yoursite
 
      2. Download and enable slick (using drush)
          $  drush dl slick
          $  drush en slick
      *When prompted click yes to make changes*

      3. Download an enable slick views
          $ drush dl slick_View
          $ drush en slick_views
    
      4. Downloand and enable Field group
          $ drush dl field_Group
          $ drush en field_group
    
      5. Download and enableField States UI
         $ drush dl field_states_ui
         $ drush en field_states_ui
 
      6. Download and enable Blazy UI
         $ drush dl blazy
         $ drush en blazy
         
      7. Add Content types(ie.)
          campus
          start cicle
          End cicle
    
      8.  Add views (this will make a page view)
          1. Disable unneded views
           2. add a view (ie. slides,videos,pages)
   
      9.  configure module slick 
          1.Add a new slick optionset
          2.name slick optionset 
          3.configure optionshet to desire settings.
    
      10.  Setup user roles(ie)
          Editors
          managers
          user permision 
      
#4. Raspberry Pi Digital Signage 

How to set up raspberry pi in kiosk mode

1. Setting up Rasbian OS If you dont have a Rasbian OS settup on the raspberri pi, click on the link below for a tutorial

        https://www.raspbian.org/FrontPage

2. Update rasbian package list

 $ ~sudo apt-get update && sudo apt-get upgrade -y
3. Install the chromium browser, x11 server utilities and unclutter(removes the cursor from the screen)

        $ ~sudo apt-get install chromium 
        $ ~sudo apt-get install x11-xserver-utils      
        $ ~sudo apt-get install unclutter 
4. Setup up SSH on raspberry pi Make sure that the pi is pluged in and connected to the network. *in raspberri pie terminal

    $ ~ifconfig

             *Write down address, netmask, and gateway.*
5. Now in terminal we are going to edit your network settings and setup the static address.

             $ ~sudo nano /etc/network/interfaces
6. This will open up your network interfaces file, start by changing the line that says

         iface eth0 inet manual
         to
         iface eth0 inet static
7. Now you’ll need to add 3 lines straight after, if they aren’t already there.

         address xxx.xxx.xxx.xxx
         netmask 255.255.255.0
         gateway xxx.xxx.xxx.xxx

          hit ctrl-O to write the file and then ctrl+X to get yourself back to your terminal screen.
8. Now that you have a static address set you can enable SSH on the pi. We’ll use the wizard that comes with raspbian.

         $ ~sudo raspi-config
      *change your user password. The default username is pi and the default password is raspberry.
9. head to option 5 – Advanced Options and then option P2 – SSH and just hit enabled.

10. test that SSH is working in an other computer

          $  ~ssh pi@xxx.xxx.xxx.xxx
Where the X’s represent the static address you gave your Pi in the previous part. If it’s worked you should be asked                          for your password. Enter the password you chose for your Pi.

11. Setting up Kiosk mode disable the screensaver and any energy saving settings (while connected to pi over SSH)

        $ ~sudo nano /etc/xdg/lxsession/LXDE/autostart

12. To disable the screensaver add a # to the beginning of the line, this comments the line out.

        @xscreensaver -no-splash
13. Add the following underneath the screensaver line to disables power management settings and stops the screen blanking after a period of inactivity

        @xset s off
        @xset -dpms
        @xset s noblank
14. To prevent any error messages displaying on the screen in the instance that someone accidentally power cycles the pi without going through the shutdown procedure.

        @sed -i 's/"exited_cleanly": false/"exited_cleanly": true/' ~/.config/chromium/Default/Preferences
tells chromium to start and which page to load once it boots without error dialogs and in Kiosk mode. replace page-to.display with whatever page you want to load.

        @chromium --noerrdialogs --kiosk http://www.page-to.display --
15. Exit File

    Hit ctrl-O and then ctrl-X again to write out and exit the file
16. Reboot pie
      
      $ ~sudo reboot
      
#4Trouble Shoot

Kiosk Does not boot into kiosk mode

1. Create a new .desktop file in ~/.config/autostart/, e.g.

      sudo nano ~/.config/autostart/autoChromium.desktop
      
2. add the following: and change testSite.com

      [Desktop Entry]
      Type=Application
      Exec=/usr/bin/chromium-browser --noerrdialogs --disable-session-crashed-bubble --disable-infobars --kiosk www.testSite.com
      Hidden=false
      X-GNOME-Autostart-enabled=true
      Name[en_US]=AutoChromium
      Name=AutoChromium
      Comment=Start Chromium when GNOME starts
      
3.reboot pi

    $ ~sudo reboot
