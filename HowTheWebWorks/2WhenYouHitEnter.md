# When you hit 'Enter'

  - To pick a zero point, let's choose the Enter key on the keyboard hitting the bottom of its range. At this point, an electrical circuit specific to the enter key is closed (either directly or capacitively). This allows a small amount of current to flow into the logic circuitry of the keyboard, which scans the state of each key switch, debounces the electrical noise of the rapid intermittent closure of the switch, and converts it to a keycode integer, in this case 13. The keyboard controller then encodes the keycode for transport to the computer. This is now almost universally over a Universal Serial Bus (USB) or Bluetooth connection.

  - In the case of the USB keyboard:
 
    - The keycode generated is stored by internal keyboard circuitry memory in a register called "endpoint".    
    - The host USB controller polls that "endpoint" every ~10ms, so it gets the keycode value stored on it.    
    - This value goes to the USB SIE (Serial Interface Engine) sent at a maximum speed of 1.5 Mb/s (USB 2.0).
    - This serial signal is then decoded at the computer's host USB controller, and interpreted by the computer's Human Interface Device (HID) universal keyboard device driver.
    - The value of the key is then passed into the operating system's hardware abstraction layer.
   
  - In the case of touch screens: 
    
   - When the user puts their finger on a modern capacitive touch screen, a tiny amount of current gets transferred to the finger. This completes the circuit through the electrostatic field of the conductive layer and creates a voltage drop at that point on the screen. The screen controller then raises an interrupt reporting the coordinate of the 'click'.    
   - Then the mobile OS notifies the current focused application of a click event in one of its GUI elements (which now is the virtual keyboard application buttons).   
   - The virtual keyboard can now raise a software interrupt for sending a 'key pressed' message back to the OS.   
   - This interrupt notifies the current focused application of a 'key pressed' event.


  
